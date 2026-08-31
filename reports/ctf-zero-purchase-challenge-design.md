# Zero-Purchase Business-Logic CTF Challenge Design

**Author:** @OWWWX-WQ  
**Discipline:** Web business logic and secure exercise design  
**Environment:** Explicitly authorized test product and synthetic transaction data  
**Publication status:** Sanitized design case study  
**Design period:** August 2026

## Overview

This case study describes the design of a CTF exercise centered on a zero-purchase or payment-bypass business-logic scenario. The exercise was intended to measure whether participants could reason about server-side price authority, payment callback authenticity, discount state, order completion, and protected digital delivery without interacting with real customer data or funds.

This is a challenge-design report, not a disclosure of a current live vulnerability. The original host, product identifier, authorization number, flag, and solution payloads are omitted.

## Learning Objectives

Participants were expected to demonstrate the ability to:

- distinguish frontend display values from server-authoritative price and quantity;
- map a multi-step cart, checkout, payment, order, and delivery workflow;
- test whether a payment result is independently verified;
- identify improper trust in callback fields or client-controlled totals;
- reason about coupon, credit, discount, and negative-value boundaries;
- use controlled evidence and stop after obtaining the challenge flag.

## Authorized Scope

The challenge was limited to:

- one designated synthetic product;
- organizer-issued test identities;
- virtual order and inventory data;
- the designated cart, checkout, payment, and order-detail paths;
- low-rate testing with no availability impact.

The rules prohibited real payment, real fulfillment, unrelated products, third-party systems, internal networks, public disclosure of the flag, and access to non-test customer data.

## Intended Hypothesis Set

The internal solution rubric considered several possible business-logic classes:

1. client-controlled price, amount, or quantity;
2. insufficient callback authenticity or status validation;
3. coupon, points, or negative-discount boundary errors;
4. access to privileged discount or internal checkout functions;
5. an order-completion chain that releases the flag only after a valid zero-value test transaction.

These were candidate solution classes for exercise construction. They were not all represented as confirmed vulnerabilities in the public design.

## Safety Engineering

- The product carried no real inventory or monetary value.
- The flag was separated from any genuine customer delivery data.
- Test identities and synthetic records were used throughout.
- Logging and rate limits were required before opening the exercise.
- Organizers retained the ability to rotate the flag and invalidate test data.
- Participants were instructed to stop after proof and avoid repeated fulfillment effects.
- Authorization and test boundaries were included directly in the challenge description.

## Challenge Flow

1. Participant receives the written authorization and designated product.
2. Participant records the normal cart and checkout flow.
3. Participant tests one variable at a time across price, quantity, discount, callback, and order state.
4. The backend releases a flag only for a qualifying synthetic completed order.
5. Participant submits the flag through the CTF platform.
6. Organizer reviews logs and resets test state where necessary.

## Judging Standard

A solution was accepted only when the participant produced a zero-value synthetic completed order and submitted the corresponding flag. A visual price change, frontend-only modification, server error, or unpaid real order was not sufficient.

## Pre-Event Smoke Test

The organizer checklist required:

- designated product page available and clearly marked as test-only;
- organizer-issued account able to complete the normal synthetic flow;
- cart, checkout, order, and payment states observable;
- flag released only by the intended server-side condition;
- CTF platform accepts the configured flag;
- rate limits and logging enabled;
- no real funds, inventory, email recipient, or fulfillment dependency;
- attachments and hints contain no source code, callback secret, or operational credential;
- reset and flag-rotation procedures verified.

## Professional Takeaways

The exercise demonstrates that business-logic security cannot be assessed only with payload scanners. A useful challenge must model authoritative state, explicit trust boundaries, independent payment evidence, negative controls, and a clear success condition. It must also prevent the training environment from becoming a reason to test unrelated real systems.

## Publication Integrity

The original design and authorization materials are retained offline. This public case study contains no target identity, flag, account, product identifier, payment secret, or reproducible live exploit chain.

