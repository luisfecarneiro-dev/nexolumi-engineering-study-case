# 03 — Architecture decisions (public summaries)

These are original summaries of the motivations and trade-offs recorded during development. They are not verbatim copies of proprietary internal ADRs.

## ADR-01: Prefer durable messaging intentions over fire-and-forget sends

**Context:** External providers can time out or acknowledge a request before downstream processing completes. Tying message delivery entirely to the UI request creates ambiguous outcomes.

**Decision:** Represent a send request as persisted work, separate acceptance from delivery, and use asynchronous processing with explicit states.

**Benefits:** Recoverability, retry visibility and clearer state transitions.

**Costs:** Worker infrastructure, monitoring, claim coordination and duplicate-effect prevention.

**Evidence boundary:** The project's private release reports document observed outbox behavior and message delivery samples. This case study does not claim zero delivery failures.

## ADR-02: Enforce tenant boundaries through authorization and RLS

**Context:** Customer data must remain separated between independently managed workspaces.

**Decision:** Use tenant-aware application authorization and database row-level policies on tenant-owned data, with verification for cross-tenant access.

**Benefits:** Defense in depth and reduced blast radius of accidental query mistakes.

**Costs:** Policy complexity, careful handling of elevated privileges and the need for database-level tests.

## ADR-03: Preserve media kind from the original message contract

**Context:** MIME types alone are not enough to identify business-level message semantics (e.g., voice note versus generic audio).

**Decision:** Persist an explicit semantic media type supplied by the relevant source envelope and avoid filling ambiguous kinds from MIME guesses.

**Benefits:** Consistent frontend rendering, lower classification ambiguity, more reliable migration/backfill rules.

**Costs:** Additional contract validation and compatibility work across adapters and persisted records.

**Observed example:** During a release review, a path persisted media without the canonical classification. The flow and migration rules were changed, then checked with focused tests and hosted evidence.

## ADR-04: Treat external events as possibly duplicated or reordered

**Context:** Webhooks, job queues and client retries may deliver events more than once or after other events have advanced state.

**Decision:** Normalize identifiers, apply idempotency and enforce valid transitions for operations that mutate durable state.

**Benefits:** Safer reprocessing and fewer duplicate side effects.

**Costs:** More persistent bookkeeping and careful semantics for deduplication windows.

## ADR-05: Use release gates with explicit NO-GO outcomes

**Context:** Local tests do not substitute for working provider credentials, production readiness or user-device behavior.

**Decision:** Separate code-quality gates, runtime checks, security checks, realistic provider workflows, soak monitoring and final pilot acceptance. Publish no Stable claim while mandatory gates are incomplete.

**Benefits:** More honest quality signal and clear next steps.

**Costs:** More verification work and potentially slower launch.

## ADR-06: Keep billing and AI behind clearly bounded integrations

**Context:** Third-party availability and provider-specific contracts can change independently of the CRM.

**Decision:** Concentrate integration-specific behavior in adapters, validate events and outcomes, and expose explicit degraded/error states to internal workflows.

**Benefits:** Better maintainability and controlled error handling.

**Costs:** Adapter maintenance and contract tests.

