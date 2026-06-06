# Stratum

Stratum is a self-hosted system design workspace for enterprise teams.

It brings architecture diagrams, requirements, journeys, documentation, reviews, versions, access control, and structured evaluation into one place so system knowledge does not get scattered across whiteboards, documents, tickets, meetings, and chat threads.

> Stratum is designed for teams that need architecture context to stay useful after the design review is over.

## Current Distribution Plan

Stratum product source code is not planned for public release at this stage.

The product is planned to be free to host through published Docker images and binaries. This `about-stratum` site exists so teams can understand the product, evaluate the deployment model, and follow setup guidance without requiring source access.

| Area | Status | Notes |
| --- | --- | --- |
| Self-hosted runtime | Planned | Docker image and binary distribution path |
| Public source code | Not planned | Product code remains private for now |
| Public docs site | In progress | This folder will later become a standalone GitHub Pages repo |

## What Stratum Solves

Most architecture knowledge is split across tools:

- Diagrams explain structure but not intent.
- Requirements are written separately from the design.
- Request flows are explained in meetings and then forgotten.
- Reviews happen in chat or ticket comments without clear version state.
- Security, availability, consistency, and traffic assumptions are reviewed too late.
- Shared enterprise systems are repeatedly redrawn instead of reused as canonical assets.

Stratum treats the design as a living system model.

```mermaid
flowchart LR
  Requirements["Requirements and NFRs"] --> Design["Structured design canvas"]
  Design --> Journeys["Journeys and request flows"]
  Design --> Docs["Design docs"]
  Design --> Review["Reviews and comments"]
  Journeys --> Review
  Docs --> Review
  Review --> Versions["Version lifecycle"]
  Versions --> Analysis["Math plus AI evaluation"]
```

## Core Features

| Feature | What it enables |
| --- | --- |
| Requirements brief | Capture use case, RPS, consistency, availability, SLA, functional requirements, and NFRs where analysis can use them. |
| Structured canvas | Model services, data stores, queues, clients, cloud frames, linked designs, and shared enterprise assets. |
| Journeys | Explain synchronous calls, async flows, callbacks, and user or system request paths. |
| Docs | Keep design documentation beside the architecture model. |
| Version lifecycle | Move versions through draft, review, reviewed, and live states. |
| Review workflow | Request reviews, track reviewer status, and keep comments tied to the design. |
| Enterprise catalog | Reuse canonical services and infrastructure across designs to reduce duplicate architecture assets. |
| Access control | Manage workspace and design-level permissions with users and groups. |
| Admin console | Configure users, sign-in, SSO, storage, AI, integrations, and platform settings. |
| Analysis engine | Run deterministic topology, traffic, reliability, and risk checks first, then optional AI synthesis when configured. |
| Self-hosted deployment | Keep architecture data inside the organization. |

## Product Surfaces

The public site avoids depending on raw application screenshots as the main marketing asset. Product UI will keep changing, and narrow development screenshots do not always present well on a polished landing page.

Instead, the site explains the main surfaces that Stratum brings together:

| Surface | Purpose |
| --- | --- |
| Requirements | Capture use case, traffic, consistency, availability, SLA, FRs, and NFRs. |
| Canvas | Model components, dependencies, cloud boundaries, linked designs, and shared assets. |
| Journeys | Explain request paths, async flows, callbacks, failure paths, and walkthroughs. |
| Reviews | Track reviewer participation, comments, and version state. |
| AI analysis | Combine deterministic checks with enterprise-configured AI synthesis. |
| Admin controls | Manage users, ACLs, SSO, storage, catalog governance, and integrations. |

## Product Model

Stratum is organized around four primary concepts:

| Concept | Description |
| --- | --- |
| Workspace | A team, platform, domain, or program boundary. |
| Design | A structured architecture model inside a workspace. |
| Version | A saved design state with lifecycle status. |
| Journey | A guided traversal that explains how a request or event moves through the design. |

## Setup Guides

The detailed guides below describe the intended configuration model. Exact image names, binary names, and final environment variable names may change before public distribution.

<details>
<summary><strong>Run in stateless mode</strong></summary>

Stateless mode is for trials, demos, and quick internal evaluation.

In stateless mode, Stratum runs without a database. Data is kept in process memory or cache-backed storage depending on the runtime configuration.

Recommended for:

- First-time product evaluation.
- Short-lived demos.
- Local experiments.
- Internal proof-of-concept sessions.

Important behavior:

- Designs and changes may not survive process restart.
- A persistent warning banner should remain visible in the app.
- Admin setup can still be completed for the running instance.
- Migration to a database-backed mode should be supported when persistent storage is configured.

Example preview command:

```bash
docker run -p 8080:8080 chaosphere/stratum
```

</details>

<details>
<summary><strong>Configure PostgreSQL storage</strong></summary>

PostgreSQL mode is the recommended production mode.

Recommended for:

- Persistent workspaces and designs.
- Enterprise user management.
- Version and review history.
- Audit-oriented deployments.
- Long-running team usage.

Expected setup flow:

1. Open the Admin Console.
2. Go to storage settings.
3. Enter the PostgreSQL connection string.
4. Test the database connection.
5. Apply the storage configuration.
6. Optionally migrate users and configuration from stateless mode.

Example configuration shape:

```bash
STRATUM_STORAGE=postgres
STRATUM_DATABASE_URL=postgres://stratum:change-me@postgres:5432/stratum
```

Operational notes:

- Use a dedicated database user.
- Store credentials in the deployment secret manager.
- Use managed PostgreSQL where available.
- Enable backups before production use.
- Keep database migrations part of the deployment process.

</details>

<details>
<summary><strong>Configure AI evaluation</strong></summary>

AI evaluation is centrally managed from the Admin Console.

Stratum should always run deterministic checks first. AI is an optional synthesis layer that can review the structured design, requirements, topology, traffic assumptions, consistency choices, availability targets, and security posture.

Expected setup flow:

1. Open the Admin Console.
2. Go to AI suite.
3. Enable AI analysis.
4. Select the provider.
5. Configure model, base URL, and API key.
6. Test the provider connection.
7. Save the configuration.

Design principle:

- The product controls the analysis prompt.
- The enterprise controls the provider and credentials.
- API keys should be stored in backend configuration, never browser local storage.
- AI output should be returned as structured findings that are safe for UI rendering.

</details>

<details>
<summary><strong>Configure Okta and SSO</strong></summary>

Stratum is designed to support local sign-in and enterprise SSO.

For Okta-style OIDC configuration, the Admin Console should capture:

- Issuer URL.
- Client ID.
- Client secret.
- Sign-in redirect URI.
- Sign-out redirect URI.
- OIDC scopes.
- Groups claim.
- Admin group mapping.
- Reviewer or architect group mapping.

Expected behavior:

- When SSO is enabled, user passwords are managed by the identity provider.
- Local password reset actions should be disabled for SSO-managed users.
- Groups from OIDC claims can map into Stratum groups.
- ACLs can grant workspace or design access to users or groups.

Recommended scopes:

```text
openid profile email groups
```

</details>

<details>
<summary><strong>Use the Admin Console</strong></summary>

The Admin Console is the enterprise command surface for Stratum.

Planned sections:

| Section | Purpose |
| --- | --- |
| Dashboard | Deployment health, setup progress, and operational state. |
| Users and roles | Local users, roles, password reset links, and account status. |
| Access center | Workspace and design ACLs for users and groups. |
| Workspaces | Workspace ownership, deletion policy, and design creation controls. |
| Enterprise catalog | Shared services and infrastructure reused across designs. |
| Sign-in and SSO | Local sign-in, Okta/OIDC, claims, and provisioning settings. |
| AI suite | Central model provider for analysis, vision, and future AI workflows. |
| Integrations | MCP readiness and future integration endpoints. |
| Security and audit | Policy posture, audit readiness, and security controls. |
| System settings | Storage mode, deployment defaults, and platform-level configuration. |

</details>

<details>
<summary><strong>Use design versions and reviews</strong></summary>

Stratum is moving toward manual version creation.

Expected lifecycle:

```mermaid
stateDiagram-v2
  [*] --> Draft
  Draft --> PendingReview: Ready for review
  PendingReview --> Reviewed: All required reviewers complete
  Reviewed --> Live: Mark live
  Live --> Draft: New changes
```

Rules:

- A new version starts as draft.
- A version can be marked ready for review only after reviewers are selected.
- Reviewers should have per-version review state.
- Only reviewed versions should be eligible to become live.
- Only one version of a design can be live at a time.
- Older versions should be viewable in read-only mode.

</details>

<details>
<summary><strong>Use journeys and docs</strong></summary>

Journeys explain how a system behaves.

Use journeys for:

- Primary user request paths.
- Async event flows.
- Callback chains.
- Failure paths.
- DR or failover procedures.
- Review walkthroughs.

Docs capture deeper context:

- Design rationale.
- Open questions.
- Runbook references.
- Data contracts.
- Security decisions.
- Capacity assumptions.
- Tradeoffs and rejected options.

Together, journeys and docs make architecture review repeatable without requiring the original author to explain every diagram live.

</details>

## GitHub Pages Site

This folder is intentionally static and can be published directly with GitHub Pages.

Files:

- `index.html`: single-page product site.
- `styles.css`: visual system and responsive layout.
- `script.js`: small progressive-enhancement interactions.

Local preview:

```bash
cd about-stratum
python3 -m http.server 8088
```

Then open:

```text
http://127.0.0.1:8088
```

## Watermark

Product belongs to Chaosphere Labs.
