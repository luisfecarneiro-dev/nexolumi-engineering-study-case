# 08 — Technical interview walkthrough

This document gives recruiters and engineering interviewers a practical way to evaluate the case study without asking for proprietary code.

## Five-minute walkthrough

1. **Product and scope (1 min):** Why a multi-tenant CRM with messaging creates more complexity than a static website.
2. **Architecture (1 min):** Explain the separation of application services, persistence, event handling and background delivery.
3. **One real debugging story (1 min):** Explain how inconsistent media-kind persistence was diagnosed and why MIME-only inference was unsafe.
4. **Security boundaries (1 min):** Explain the distinction between workspace authorization and row-level policies.
5. **Release trade-offs (1 min):** Explain why local green checks did not justify a Stable release declaration.

## Questions I can discuss

**How would you prevent duplicate side effects after a retry?**  
Start with a stable command identifier, idempotent transitions, durable state and explicit reconciliation for unknown provider outcomes. Explain where at-least-once delivery is acceptable and where "exactly once" should not be promised.

**What is the difference between authentication, authorization and tenant isolation?**  
Authentication establishes the user identity; authorization governs allowed operations; tenant isolation ensures requests cannot access records belonging to another organization, including at the database boundary.

**Why not use MIME type alone for media?**  
Because a generic audio MIME does not tell us whether a recording is a voice note. That semantic distinction must come from a trusted, validated source contract.

**What does a passing build prove?**  
It proves that a specific build completed under stated conditions, not that providers, runtime permissions, sessions, UX or security checks are all correct.

**What would you prioritize before shipping?**  
Critical messaging flows, tenant boundaries, billing state, mobile usability, runtime readiness, the CI gate, extended monitoring and a real pilot.

## Suggested evaluation exercise

Ask the candidate to design a small message queue domain from scratch, covering a durable outbox, a retry state machine, idempotency, test cases and failure reporting. The example should be independent of NexoLumi source code.

## Disclosure boundary

This portfolio artifact documents reasoning and high-level evidence. An employer may request a live demonstration or a fresh coding exercise; private code, customer data and credentials are not distributed through this repository.

