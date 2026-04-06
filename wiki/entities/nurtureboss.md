---
title: NurtureBoss
type: organization
entity_type: startup, case study
sources: 1
---

# NurtureBoss

## Overview

AI startup building a leasing assistant for apartment property managers. Serves as the primary case study in [[Hamel Husain]]'s guide to LLM evaluations, demonstrating the transition from vibe-based development to systematic evaluation.

## Product

AI-powered leasing assistant that helps with:
- Tour scheduling
- Answering routine tenant questions
- Inbound sales for apartment properties

## Key People

- [[Jacob]] — Founder, served as domain expert for evaluation labeling

## Technical Approach

- Built sophisticated agent for property management conversations
- Initially struggled with "vibes-based development" (LGTM testing)
- Adopted systematic [[Error Analysis]] workflow
- Built custom data viewer for efficient trace review
- Developed both [[Code-Based Evals]] (for date handling) and [[LLM-as-Judge]] (for handoff decisions)

## Identified Failure Modes

Through error analysis, discovered three primary issues:
1. Date handling errors
2. Handoff failures (not transferring to humans appropriately)
3. Conversation flow issues

## Website

https://nurtureboss.io/

## Referenced In

- [[A pragmatic guide to LLM evals for devs]]
