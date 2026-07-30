# Datacern AI — Platform Narrative (GTM source of truth)

This document is the single source of truth for how Datacern is positioned.
The website, decks, and partner material derive from it. Every claim below is
tied to shipped, CI-verified capability in the platform repository
(`Datacern_AI_V0.1`) — the "Proof" column names where it is enforced or
tested. Anything not provable stays in the Vision section, clearly marked.

---

## 1. Category thesis

Enterprises run on two kinds of systems: **systems of record** (where
operational data lives) and **systems of insight** (where analysts look at
it). Between them sits the actual work of a regulated operation — disputes,
claims, alerts, losses, invoices — where a person must look at live data and
make a decision they can defend to an auditor. Today that layer is email,
spreadsheets, swivel-chairs, and increasingly, ungoverned AI experiments.

**Datacern is the system of decision**: the layer that turns live
operational data into a worked, signed, defensible decision — and learns
from every signature.

One line: **Live data in. Defensible decisions out. Every decision makes the
next one cheaper.**

## 2. The narrative spine — three verbs

Everything the platform does hangs off three verbs. Say them in this order,
always:

| Verb | What it means | What ships behind it |
|---|---|---|
| **Stream** | Watch the systems that already hold the work; new and changed rows become cases on the right department's worklist, evidence frozen at intake | 23 real source drivers (Postgres, MySQL, SQL Server, Oracle, Snowflake, BigQuery, Databricks, Redshift, Salesforce, S3/GCS/Azure, SFTP…), watermark-incremental case streams (priced add-on), intake-snapshot evidence, workspace-scoped triggers |
| **Decide** | AI drafts the recommendation with its evidence; a named human signs; no one approves their own work; every step leaves a receipt | Agent proposals, four-eyes approval, signed execution grants, tool-plane authorization, hash-chained WORM audit, SLA policies, dispositions |
| **Learn** | Every signed decision becomes a training label; models retrain in governed pipelines, promote under four-eyes, and pre-score the next batch | `case.disposition_applied` label events → labeled datasets → training pipelines → MLflow-tracked experiments → promotion approval → scheduled scoring → score-triggered cases |

The accountability signature — **"AI does the work. A named human signs
it."** — stays. It is the memorable line inside the Decide verb, not the
whole story.

## 3. Messaging hierarchy

- **H1 (category):** The system of decision for regulated operations.
- **H2 (signature):** AI does the work. A named human signs it.
- **H3 (flywheel):** Stream → Decide → Learn. Every decision makes the next
  one cheaper.
- **Proof beat (always paired with any AI claim):** every step leaves a
  receipt an examiner can follow; every change runs live end-to-end journeys
  in CI that assert the loop on real state, and merges wait for green.
  (Say "CI refuses to merge" only once branch protection on `main` requires
  the `e2e-live` check — enable it in repo Settings; until then it is
  practice, not mechanism.)

## 4. Land → expand motion (implemented, not aspirational)

1. **Land on one queue.** A vertical pack (28 shipped: card disputes, health
   claims, financial-crime alerts, insurance losses, supplier invoices…)
   plus a demo sandbox and self-service trial puts a working, governed queue
   in front of a buyer in days.
2. **Expand across departments.** Workspaces are department boundaries
   enforced at the API (cross-department requests 404); one tenant hosts
   many departments without a new deployment.
3. **Deepen with add-ons.** Realtime case streams is the first separately
   licensed SKU — entitlement-gated in code (create/resume refuse without
   it; off is never gated). The commercial plane (plans, entitlements,
   metering, trials, ROI reporting) exists precisely to price this motion.
4. **Compound.** The Learn loop pre-triages tomorrow's queue with models
   trained on the customer's own signed decisions — switching cost grows
   with every signature.

## 5. Competitive frame

| Against | Their gap | Our line |
|---|---|---|
| BI / analytics | Shows the problem, doesn't work it; no accountability chain | "A dashboard has no signature." |
| Workflow/ticketing (ServiceNow-class) | Routes work; doesn't draft it, doesn't learn from it; AI bolted on without provable controls | "Routing isn't deciding." |
| Agent frameworks / copilots | No four-eyes, no grants, no receipts; unusable where money moves | "An agent that can't be audited can't be deployed here." |
| ETL/iPaaS | Moves data; produces no case, no decision, no defense | "Pipelines end in tables. Ours end in signed decisions." |

## 6. Proof points (claim → where it is enforced/proven)

| Claim | Proof |
|---|---|
| New source rows become department-scoped cases with frozen evidence | `make journey-streams` in CI: seeded Postgres → stream → case in dept A with `intake_snapshot/v1` evidence bytes |
| Another department cannot even confirm the case exists | `RequireCaseWorkspace` middleware → 404 on case/timeline/evidence; asserted live in the same journey |
| Only what's new is pulled | Watermark cursor; journey asserts second fire yields exactly one new case, cursor advanced |
| Approved AI decisions actually change the row | `make journey` (governed write loop): proposal → different human approves → tool-plane grant → the case row is different |
| The add-on is commercially gated, fail-closed | 403 names the SKU; 503 `ENTITLEMENT_UNAVAILABLE` when the commercial plane can't be consulted; pause/off never gated |
| Decisions become labels; models promote under four-eyes | `case.disposition_applied` events → pipeline labeled datasets; promotion approval in experiment-service; retrain leg runs in CI seed |
| Tenant isolation is structural | Postgres FORCE row-level security on every tenant table |
| Every action leaves an examiner-grade trail | Hash-chained audit with WORM storage; cross-workspace denials emit their own audit event |
| Compliance posture is mapped, not asserted | Pack control mappings to EU AI Act, NIST AI RMF, ISO 42001 |

## 7. What we deliberately do NOT claim

- **Milliseconds.** Streams are watermark polls (60s minimum interval).
  Freshness is seconds-to-minutes. Say it plainly; it wins trust and is
  sufficient for the work.
- **Pretrained magic.** Fraud/intent/behavior models are trained on the
  customer's own data in the governed ML factory. Packs ship the scaffolding,
  not a black box.
- **Voice, telephony, or RPA.** Integrations land data; Datacern makes it
  decidable and defensible.
- **DB-level department isolation.** Tenants are isolated by RLS;
  departments by an API-enforced workspace boundary. Both true, stated
  separately.

## 8. Vision horizons (label as vision when speaking)

- **Now (shipped):** one platform, 23 services, 28 packs — live data to
  signed decisions to learning models, gated by CI journeys.
- **Next (roadmap):** push/streaming sources (Kafka-as-a-source), richer
  per-workspace analytics, more scored verticals, deeper POC-to-value
  reporting.
- **Long-term:** the enterprise's **decision memory** — the accumulated,
  auditable corpus of who decided what, on what evidence, with what outcome —
  as the substrate no competitor can replicate, because it is earned one
  signed decision at a time.
