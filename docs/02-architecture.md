# 02 — High-level architecture

## System overview

```mermaid
flowchart LR
    Browser[Browser / PWA] --> Web[Next.js / React application]
    Web --> Data[(PostgreSQL / Supabase)]
    Web --> Messaging[Messaging domain]
    Web --> Billing[Billing adapter]
    Provider[External messaging services] --> Incoming[Verified incoming events]
    Incoming --> Messaging
    Messaging --> Data
    Messaging --> Queue[(Durable message outbox)]
    Queue --> Worker[Delivery worker]
    Worker --> Provider
    Messaging --> AI[Optional AI service]
```

The diagram is intentionally schematic and omits infrastructure-level details.

## Separation of concerns

**Web application:** rendering, navigation, authenticated workflows, workspace context, user-initiated commands, human-readable error states.

**Domain/application services:** operations such as adding a contact, changing a deal, initiating an outbound message or interpreting an incoming event; responsible for validation, authorization and transaction boundaries.

**Data layer:** relational persistence; explicit workspace ownership; policies that constrain access according to user identity and permitted memberships. RLS supplements server-side authorization rather than replacing it.

**Messaging adapters:** normalize provider-specific inbound/outbound contracts while retaining necessary provider context. Protect internal workflows from schema differences.

**Asynchronous workers:** execute work that should survive a web request lifecycle; preserve retry and diagnostic state.

**Integration adapters:** billing and AI dependencies are treated as external systems whose responses must be validated and whose failures must be visible.

## Example: outbound message lifecycle

```mermaid
sequenceDiagram
    participant Agent as CRM user
    participant App as App/API
    participant DB as Database
    participant Worker as Outbox worker
    participant Channel as Message provider
    Agent->>App: Request message send
    App->>App: Authenticate, authorize, validate
    App->>DB: Persist message intention + outbox record
    DB-->>App: Commit / identifier
    App-->>Agent: Accepted for processing (not delivered)
    Worker->>DB: Claim pending work
    Worker->>Channel: Send via provider adapter
    Channel-->>Worker: Response or failure
    Worker->>DB: Record outcome / retry state
```

*Illustrative flow based on system design. Actual delivered status requires provider evidence; queuing is not equivalent to delivery.*

## Architectural trade-offs

- **Asynchronous processing** improves recoverability but adds coordination, observability and duplicate-handling requirements.
- **Multiple providers** increase interoperability and operational complexity.
- **RLS** can add defense in depth, but policy correctness and service-role boundaries must be continuously audited.
- **PWA/mobile** improves accessibility of the product but requires explicit handling for cached state and offline limitations.

## Scope boundaries

No private paths, specific production service hosts, production identifiers, exact permission predicates, network configurations or service credentials are included.

