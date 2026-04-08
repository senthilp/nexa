---
title: "Optimizing Vercel Sandbox snapshots"
source: "https://vercel.com/blog/optimizing-vercel-sandbox-snapshots"
author:
  - "[[Tom Lienard]]"
  - "[[Rob Herley]]"
  - "[[Luke Phillips-Sheard]]"
  - "[[Guðmundur Bjarni Ólafsson]]"
published: 2026-04-02
created: 2026-04-08
description: "Vercel Sandbox snapshots let you save and restore your entire filesystem. Learn how we optimized snapshot restores with parallel downloads, streaming decompression, and local NVMe caching."
tags:
  - "clippings"
---
When we recently shipped [filesystem snapshots](https://vercel.com/changelog/filesystem-snapshots-supported-on-vercel-sandboxes) in Vercel Sandbox to let teams capture and restore a sandbox's entire filesystem state, our initial engineering focus was entirely on reliability, making sure the system would never fail to snapshot or lose data.

Once that foundation was stable, our attention turned to performance. p75 snapshot restores were taking over 40 seconds, and through parallelization and local caching, we brought that under one second.

Vercel Sandbox runs on the same infrastructure as our internal builds product, [Hive](https://vercel.com/blog/a-deep-dive-into-hive-vercels-builds-infrastructure). Each sandbox is an isolated container inside a Firecracker microVM.

A snapshot is a compressed copy of the sandbox's disk. We're working with two different files:

- The raw disk image (
	```
	.img
	```
	), which can be several GBs
- A compressed version in our custom
	```
	VHS
	```
	format (Vercel Hive Snapshot), which is what gets uploaded to and downloaded from S3

When you call

```
sandbox.snapshot()
```
, we compress the
```
.img
```
into a
```
.vhs
```
and upload it to S3. When you call
```
Sandbox.create()
```
with a snapshot, we download the
```
.vhs
```
and decompress it back. Without compression, every snapshot operation transfers hundreds of MBs to low GBs over the network, adding seconds to tens of seconds to every restore.

With reliability in place, we turned to the restore path, which was painfully sequential. We'd download the entire

```
.vhs
```
file from S3 in a single request, wait for it to finish, then decompress it in a single thread.

![The original restore pipeline: a single S3 download followed by single-threaded decompression](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F2ZCL5V22wYvUEB3ZPHLEFn%2F03f5fc467b2adff070d635c262dbbd04%2Fsequential-pipeline--light.png&w=1920&q=75) ![The original restore pipeline: a single S3 download followed by single-threaded decompression](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F5KgFcpRRN0rGjExM8GwZhL%2F76cfed178b3fe8ae5f8a7168e8636c05%2Fsequential-pipeline--dark.png&w=1920&q=75) ![The original restore pipeline: a single S3 download followed by single-threaded decompression](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F16yw5TgahBbo51IDcEZLBv%2F5c0ebb210068a5a3b89e60c095e213ad%2Fdiagram-name--mobile-light.png&w=1920&q=75) ![The original restore pipeline: a single S3 download followed by single-threaded decompression](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F1ECiJbeGDWfTx3iyP8YLCl%2F1c6ec647f3345b7be9bd6a8e2ec1de3e%2Fdiagram-name--mobile-dark.png&w=1920&q=75)

The original restore pipeline: a single S3 download followed by single-threaded decompression

Snapshots range from 200MB to a few GBs, so that single S3 download alone could take several seconds to tens of seconds. We used the

```
Range
```
HTTP header to [download chunks in parallel](https://docs.aws.amazon.com/AmazonS3/latest/userguide/optimizing-performance-guidelines.html) instead, with the AWS Go SDK's
```
transfermanager
```
API handling the orchestration. After benchmarking different concurrency levels and chunk sizes, we ended up with 2-5x faster downloads.

![Splitting the download into parallel S3 range requests](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2FnL6pZKKW4rMUK2J1odmH6%2Fe8443d7dc2b1c2261adc0aef10917a9a%2Fparallel-download--light.png&w=1920&q=75) ![Splitting the download into parallel S3 range requests](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F3YgR0O3wDNnd5n5G2Z4kNr%2Fbfd74d5ea7efd7aff38c38ab2cd66644%2Fparallel-download--dark.png&w=1920&q=75) ![Splitting the download into parallel S3 range requests](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2Fr9hieJQtQ2RxxhjgLrub0%2Fc1e0e02664e1e8da3e26079abfadc72e%2Fparallel-download--mobile-light.png&w=1920&q=75) ![Splitting the download into parallel S3 range requests](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F2xZCqLrYeNETT3jqLY5Kga%2Fc98b2d5796ece06c1edf9601ec3fdcec%2Fparallel-download--mobile-dark.png&w=1920&q=75)

Splitting the download into parallel S3 range requests

We applied the same thinking to decompression. Our

```
.vhs
```
format stores a header and a frame for each allocated region of the disk image, so instead of decoding and decompressing frames one by one, we switched to one decoder feeding N decompression goroutines. That made the
```
.vhs
```
to
```
.img
```
restore 2-4x faster, depending on snapshot size.

![Fanning out decompression across multiple goroutines](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F4gqeqWpAa3TO6mLfi0H4vV%2F3778a5028e618d1e33831c7d6199653b%2Fparallel-decompress--light.png&w=1920&q=75) ![Fanning out decompression across multiple goroutines](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F3P6RmSKzLQp7tPaDUJf8QD%2F8f75248e569b89636d0cb73cfc4bce9c%2Fparallel-decompress--dark.png&w=1920&q=75) ![Fanning out decompression across multiple goroutines](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F3BxlYRJFZeytMP2Dlyw2mx%2Fbdb0628ed88d9b2c2aed156ecbe2fe97%2Fparallel-decompress--mobile-light.png&w=1920&q=75) ![Fanning out decompression across multiple goroutines](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F4WDH7GVCVYzBAwWZfEFmsX%2F1c187ee6d8111dfa1b0500dc7d60e7cb%2Fparallel-decompress--mobile-dark.png&w=1920&q=75)

Fanning out decompression across multiple goroutines

Even with both downloading and decompressing parallelized, the pipeline still wrote downloaded data to disk before decompression could begin. Piping S3 range request streams directly into decompression eliminated that intermediary step, cutting end-to-end restore time by another 2x.

![Piping S3 download streams directly into decompression, no intermediate file](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F4tzVg3vfrgyY05aRQW65zf%2Ff114c2e66653dcadc50a33c376dccd2a%2Fstreaming-pipeline--light.png&w=1920&q=75) ![Piping S3 download streams directly into decompression, no intermediate file](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F4kjztIe6Yssmj3z29WpTq3%2F55f478a609c775d30205432875bc0386%2Fstreaming-pipeline--dark.png&w=1920&q=75) ![Piping S3 download streams directly into decompression, no intermediate file](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F4eafv9vlQljqumbSKINApB%2F0120723fe8dd3022c0ff032d7a884167%2Fstreaming-pipeline--mobile-light.png&w=1920&q=75) ![Piping S3 download streams directly into decompression, no intermediate file](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F5WLBbrXdRUdUd7iKuBavYX%2F42fcbbf6e4b8e329732455083b8661cf%2Fstreaming-pipeline--mobile-dark.png&w=1920&q=75)

Piping S3 download streams directly into decompression, no intermediate file

As you might have noticed, we so far only talked about improving the slow path, when we need to retrieve a snapshot from S3 on a cache miss. Well, we actually didn't have a fast path, so it was all cache misses. Yeah, we really didn't focus on performance at first.

Our sandboxes run on metal instances with NVMe disks, giving us several terabytes of fast local storage that was mostly sitting unused. We put it to work with a local disk cache using LRU (least recently used) eviction, sized by total disk space rather than number of entries. We cache the decompressed

```
.img
```
directly rather than the compressed
```
.vhs
```
, so a cache hit skips both the download and the decompression. Once the cache fills up, the least recently used snapshots get evicted to make room.

Most customers reuse a "base" snapshot across many sandboxes, which gives us a 95% cache hit rate. On those hits, boot time is bounded only by starting the microVM and container.

![Local NVMe cache hit rate, consistently above 90%](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F34UXzDdCqNgH5AQt93SmJJ%2F6e43b4c4df8b595a9919ff69cbbd6c63%2Fcache-hit-rate--light.png&w=1920&q=75) ![Local NVMe cache hit rate, consistently above 90%](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F2xVcj4atORlsxnVqJTtS53%2F73320f213aec4e88e0052bf2e0eab699%2Fcache-hit-rate--dark.png&w=1920&q=75) ![Local NVMe cache hit rate, consistently above 90%](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F4V9zteWNzSC5XeuY7u53wl%2F918223ecb5ddb374e5b3f8b0f3db8a3b%2Fcache-hit-rate--mobile-light.png&w=1920&q=75) ![Local NVMe cache hit rate, consistently above 90%](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F5btvzSgig7PNseIH5U9Yws%2F5bfd935cfa46c817f0b1130a58f6de9c%2Fcache-hit-rate--mobile-dark.png&w=1920&q=75)

Local NVMe cache hit rate, consistently above 90%

p75 dropped from 40s to sub-second, and p95 went from 50s to 5s. With our cache hit rate, most sandbox boots skip the download and decompression pipeline entirely.

![Snapshot restore p95 latency dropping from 50s to under 10s](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F5zmyFajw2aVn0FdPGeDbh7%2F62b75074643c022bb14377acf8934c93%2Flatency-before-after--light.png&w=1920&q=75) ![Snapshot restore p95 latency dropping from 50s to under 10s](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F5JPmWBnne6UFXPOj6vKqj5%2F05bca9fe513702071930638bb8fdb9b0%2Flatency-before-after--dark.png&w=1920&q=75) ![Snapshot restore p95 latency dropping from 50s to under 10s](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F54E0ZCZDcTsAuR3psIeda7%2Fe849e5a239a239660c1f4cef46eb216e%2Flatency-before-after--mobile-light.png&w=1920&q=75) ![Snapshot restore p95 latency dropping from 50s to under 10s](https://vercel.com/vc-ap-vercel-marketing/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fcontentful%2Fimage%2Fe5382hct74si%2F3p0adKy6YG8QErlqXShIsO%2F9cdd0db8d6ec24fb2898e81dff741022%2Flatency-before-after--mobile-dark.png&w=1920&q=75)

Snapshot restore p95 latency dropping from 50s to under 10s

There's more we can do. Cache affinity, for example, would route sandboxes to metal instances that already have the requested snapshot cached, potentially eliminating the cold path for popular snapshots. But that risks creating thundering herds and hotspotting certain machines, so we're being deliberate about it.

Long term, we want the cold path fast enough that caching is a bonus, not a requirement.

These optimizations already power [Automatic Persistence](https://vercel.com/changelog/vercel-sandbox-persistent-sandboxes-beta), now in beta, which automatically snapshots a named sandbox's filesystem when you stop it and restores everything on resume. With sub-second restores, that stop-and-resume cycle feels instant.

Filesystem snapshots are available today for all Vercel Sandboxes. Check the [Sandbox documentation](https://vercel.com/docs/vercel-sandbox/concepts/snapshots) to get started.