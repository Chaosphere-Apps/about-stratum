# Stratum

**Design, explain, review, version, and evaluate system architecture in one self-hosted workspace.**

Stratum is built for engineering teams that need architecture context to survive beyond whiteboard sessions, design reviews, incident retrospectives, and scattered documentation.

Public product page: [chaosphere-apps.github.io/about-stratum](https://chaosphere-apps.github.io/about-stratum/)

---

## Why Stratum Exists

System design work is usually split across too many places:

| What teams need | Where it often ends up |
| --- | --- |
| Architecture diagrams | Whiteboards or drawing tools |
| Requirements and assumptions | Docs, tickets, chats, or meeting notes |
| Request flows | Explained verbally, then forgotten |
| Reviews and approvals | Comments scattered across tools |
| Versions and release state | Informal screenshots or duplicated diagrams |
| Shared services | Redrawn repeatedly with inconsistent names |
| Risk analysis | Manual, late, or disconnected from the actual model |

Stratum turns architecture design into a structured, reviewable, versioned system model.

```mermaid
flowchart LR
  Requirements["Requirements and NFRs"] --> Canvas["Structured design canvas"]
  Canvas --> Journeys["Journeys and request flows"]
  Canvas --> Docs["Design documentation"]
  Canvas --> Review["Reviews and comments"]
  Review --> Versions["Version lifecycle"]
  Versions --> Analysis["Math plus AI analysis"]
  Canvas --> Catalog["Enterprise catalog"]
```

---

## What You Get

### Structured System Design Canvas

Create architecture diagrams using system-aware components such as services, APIs, queues, databases, object stores, clients, cloud frames, security controls, observability components, and linked designs.

Unlike a generic drawing canvas, Stratum keeps the model structured so it can later be searched, reviewed, versioned, analyzed, and exported.

### Requirements Next To The Design

Capture the design brief where it belongs:

- use case
- functional requirements
- non-functional requirements
- target RPS
- consistency expectations
- availability target
- SLA
- assumptions and constraints

This matters because architecture analysis without requirements is mostly guesswork.

### Journeys And Request Flows

Explain how a request, event, callback, async workflow, or failure path moves through the system.

Journeys help reviewers understand the design without needing a live walkthrough every time.

### Documentation Beside The Model

Attach design documentation directly to the system workspace so diagrams and explanation evolve together.

### Manual Versions And Review Lifecycle

Stratum is designed around an explicit lifecycle:

```mermaid
stateDiagram-v2
  [*] --> Draft
  Draft --> PendingReview: Ready for review
  PendingReview --> Reviewed: Review complete
  Reviewed --> Live: Mark live
  Live --> Draft: New changes
```

Only reviewed versions should become live. Older versions can be viewed without editing the current design.

### Reviews And Comments

Request reviews from selected reviewers, capture comments, and keep review state tied to a design version.

### Enterprise Catalog

Register canonical services, infrastructure, platforms, and shared assets in an enterprise catalog. Designs can reference those shared assets instead of recreating duplicate components with slightly different names.

This helps teams reason about reuse and future impact analysis.

### Analysis Engine

Stratum is designed to combine:

- deterministic checks for topology, traffic, consistency, availability, reliability, security, and operational signals
- optional enterprise-configured AI synthesis for richer review feedback

The product controls the analysis workflow. The enterprise controls the AI provider and credentials.

### Enterprise Administration

Admin surfaces are planned around real enterprise needs:

- local users and roles
- password reset links for local accounts
- SSO/OIDC configuration
- Okta group claim mapping
- workspace and design ACLs
- user and group grants
- storage mode
- AI provider configuration
- MCP readiness
- catalog governance

---

## How To Get Started

Stratum is intended to be self-hosted. The product source code is not planned for public release at this stage, but Docker images and binaries are intended to make Stratum easy to run inside your environment.

### Option 1: Try The All-In-One Container

Use this when you want the simplest first run. The all-in-one image serves both the UI and backend from one container.

```bash
docker run --rm \
  -p 8080:8080 \
  ghcr.io/chaosphere-apps/stratum-allinone:latest
```

Then open:

```text
http://localhost:8080
```

If no database is configured, Stratum runs in stateless mode.

### Option 2: Run With PostgreSQL

Use this for persistent workspaces, designs, versions, reviews, users, and configuration.

```bash
docker run --rm \
  -p 8080:8080 \
  -e DATABASE_URL="postgres://stratum:change-me@postgres:5432/stratum?sslmode=disable" \
  ghcr.io/chaosphere-apps/stratum-allinone:latest
```

For production, use managed PostgreSQL or a dedicated Postgres container with persistent volumes and backups.

### Option 3: Run UI And Backend Separately

Use this when your deployment platform prefers independent services.

```bash
docker pull ghcr.io/chaosphere-apps/stratum-ui:latest
docker pull ghcr.io/chaosphere-apps/stratum-backend:latest
```

The UI can be served independently while the backend handles API, WebSocket, analysis, auth, storage, and administration.

---

## Deployment Modes

| Mode | Best for | Persistence |
| --- | --- | --- |
| Stateless mode | trials, demos, short evaluations | not guaranteed after restart |
| PostgreSQL mode | team usage and production | persistent |
| All-in-one image | simple self-hosted deployment | depends on configured storage |
| Split UI/backend | platform teams and larger deployments | depends on backend storage |

If Stratum is running without a database, the app should show a clear stateless-mode banner so teams know changes may not survive restart.

---

## SSO And Okta

Stratum is designed to support local sign-in and enterprise SSO.

For Okta/OIDC-style setup, administrators configure:

- issuer URL
- client ID
- client secret
- sign-in redirect URI
- sign-out redirect URI
- scopes
- groups claim
- admin/reviewer/architect group mapping

Recommended scopes:

```text
openid profile email groups
```

When SSO is enabled, passwords are managed by the identity provider and local password reset actions should be disabled for SSO-managed users.

---

## AI Analysis

AI is optional and centrally configured.

Stratum should first run deterministic checks, then use the configured model to synthesize review findings from the structured design, requirements, journeys, docs, and version context.

Expected admin configuration:

- provider
- model
- base URL
- API key
- connection test
- analysis enabled/disabled

API keys should be stored in backend configuration, never in browser local storage.

---

## Who Stratum Is For

Stratum is a fit for teams that:

- design backend systems, platforms, APIs, data flows, and distributed systems
- need design review history
- want architecture decisions to stay connected to diagrams
- need reusable enterprise service catalogs
- care about consistency, availability, traffic, security, and reliability tradeoffs
- want self-hosted control over architecture data
- want AI analysis without sending design context through unmanaged tools

---

## Current Product Direction

Stratum is focused on becoming a practical architecture workspace, not a generic whiteboard.

Near-term priorities:

- stable and clean canvas creation
- excellent version and review lifecycle
- polished journeys and flow playback
- stronger enterprise catalog usage
- robust ACLs for workspaces and designs
- production-grade storage configuration
- useful deterministic plus AI analysis

---

## Feedback And Early Access

When this public page is split into its own repository, product feedback, setup questions, reviews, and deployment requests will be handled through GitHub Issues and Discussions.

For now, use the product page as the main reference:

[https://chaosphere-apps.github.io/about-stratum/](https://chaosphere-apps.github.io/about-stratum/)

---

<p align="center">
  <strong>Stratum belongs to Chaosphere Labs.</strong>
</p>
