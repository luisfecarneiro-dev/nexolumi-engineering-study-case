# 04 — Messaging reliability: an engineering deep dive

## Why messaging was a difficult subsystem

A chat UI has more observable states than a simple "POST succeeded" operation: accepted, queued, provider-acknowledged, sent, delivered, read, failed, retried or expired. Media introduces more distinctions: format, semantic kind, storage location, permissions and rendering behavior.

## Failure model

| Failure mode | Architectural response | Verification question |
| --- | --- | --- |
| Provider unavailable | Persist send intention; retry or surface failure | Is the message still traceable? |
| Client retries request | Use stable identifiers/idempotency | Did one logical action produce duplicate sends? |
| Duplicate webhook | Deduplicate or make handlers safe to repeat | Did state change twice? |
| Media metadata missing | Validate and preserve semantic metadata | Is the displayed media kind correct? |
| Session disconnected | Reconcile state and show connection health | Does the interface claim success incorrectly? |
| Worker crashes mid-send | Recover claims and resolve ambiguous status | Are attempts bounded and observable? |
| Webhook arrives out of order | Validate legal transitions and reconcile | Is durable state internally consistent? |

## Example issue: lost semantic media classification

**Problem discovered:** An inbound processing path inserted media-related records without consistently persisting the canonical `media_kind`. Some audio/voice distinction had also been inferred from MIME data, even though MIME did not encode whether the user sent a voice note.

**Analysis:** The semantic truth belongs to the provider's message envelope or an explicit, validated client declaration. Applying a MIME heuristic can silently change the meaning of the message.

**Remediation documented in the private project:** Update ingress and outbound paths to carry a canonical media kind, constrain accepted values, use additive data correction rules and verify rendering behavior with targeted tests.

**Why the fix matters:** A voice message should not be misrepresented as a generic audio attachment. A real GIF should not depend on a video decoder. Message state must remain consistent across persistence, worker operations and UI.

**Evidence and limitation:** The internal release record includes specific passing tests and hosted observations for classification and rendering. Some other provider-dependent media scenarios remained in pilot validation as of October 11, 2026. This case study does not claim full messaging completeness.

## Reliability principles applied

1. **Persist intent before dispatch.** An accepted request is not proof of delivered communication.
2. **Prefer repeat-safe effects.** Every retry path needs a plan for duplicate attempts.
3. **Classify from source semantics.** Avoid guessing behavior from ambiguous transport data.
4. **Make failure states visible.** A silent failure is worse than a clearly reported retry.
5. **Test the boundary.** Tests should include invalid identifiers, timeouts, duplicates, out-of-order events and renderer edge cases.

## What I would evaluate next

A complete assessment should include a full soak window, provider-backed send/receive tests on real devices, session reconnect exercises, multi-workspace authorization testing, failure-injection scenarios and reconciliation of final user-visible statuses.

This document intentionally does not provide actual provider endpoints, session details, tokens or internal remediation playbooks.

