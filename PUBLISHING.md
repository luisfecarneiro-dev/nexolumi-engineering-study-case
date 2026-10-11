# Repository maintenance and publication checklist

This is the public documentation repository for the **NexoLumi CRM engineering case study**:

https://github.com/luisfecarneiro-dev/nexolumi-engineering-study-case

The commercial NexoLumi CRM source repository remains **private** and separate from this documentation. Do not copy proprietary source code, infrastructure credentials, internal provider URLs or customer records here.

## Safe-publication checklist

1. Check every added file and its metadata for personal data, access tokens, integration identifiers and private operational logs.
2. Use diagrams at a conceptual level. Never publish implementation details that would weaken production security.
3. Review screenshots against synthetic test data. Redact message contents, contacts, phone numbers, avatars and workspace identifiers.
4. Revalidate dates and claims before updating test counts, release evidence or production status. Keep the `NO-GO` status until a subsequent release gate explicitly approves `GO`.
5. Verify relative Markdown links and Mermaid diagrams in the GitHub preview.
6. Publish clear, focused documentation commits rather than importing the private product's history.

## Suggested GitHub topics

`case-study`, `software-engineering`, `saas`, `nextjs`, `typescript`, `postgresql`, `system-design`, `architecture`, `reliability`

## Portfolio usage

Pin this repository to the GitHub profile and link it from the portfolio or resume. In interviews, use `docs/08-recruiter-walkthrough.md` to explain engineering decisions and their limitations. This is a **case study**, not a runnable product clone.
