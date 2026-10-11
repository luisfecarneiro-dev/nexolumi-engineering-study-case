# 07 — Lessons learned

## 1. Feature breadth increases integration risk

Building a CRM, inbox, automation builder, billing system and AI features within one product makes contracts between domains especially important. Adding features faster than they can be validated increases the risk of defects that only appear in end-to-end workflows.

**Takeaway:** Freeze scope at release time, stabilize the critical user journeys, then reopen feature development.

## 2. Message semantics are part of the data model

The media-classification issue demonstrated that transport MIME type and application-level intent are not interchangeable.

**Takeaway:** Preserve meaningful type information from the source, validate it at ingress and carry it through persistence and rendering.

## 3. Safety relies on multiple boundaries

Application authorization and database-level RLS help address different classes of mistakes. Neither should be used as a blanket guarantee.

**Takeaway:** Test forbidden cases and elevated-privilege transitions, not just happy-path requests.

## 4. The interface must represent uncertainty

A button click, an accepted API request and a delivered WhatsApp message are three different outcomes.

**Takeaway:** Model pending, partial and failed states explicitly; avoid optimistic UI that looks like confirmed delivery when it is not.

## 5. Release discipline is an engineering skill

A passing local build is a necessary but insufficient condition for a stable product.

**Takeaway:** Keep auditable go/no-go criteria, distinguish external blockers and refuse to declare stability early.

## 6. Documentation should explain decisions, not just list dependencies

A list of libraries does not show why trade-offs were made. A useful case study describes failure models, alternatives, consequences and measurable evidence.

**Takeaway:** Structure engineering documentation around decisions, constraints and verified observations.

## Skills demonstrated

TypeScript/React full-stack development; relational data modeling; web-service integration; backend reliability; authentication and authorization reasoning; incident diagnosis; test design; release planning; technical writing; product ownership.

## Remaining improvement areas

Formal peer review within a larger engineering team, production observability under higher traffic, completing final provider-backed pilot cases, finishing stable-release gates and measuring real-world performance across devices.

