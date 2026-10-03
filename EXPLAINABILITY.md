# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **# Explainability & Decision Transparency Report** (`agentic-inbox`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** # Explainability & Decision Transparency Report (`agentic-inbox`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Productivity / Autonomous Email Management, Inbound Routing & Drafting  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

### 1. Deterministic Multi-Stage Decision Pipeline
The agent manages incoming correspondence, thread reasoning, and reply composition through a deterministic, 5-stage pipeline on Cloudflare Workers.

### 1. Decision Architecture

The runtime intake, state classification, evaluation, and execution tracking operate across a deterministic, five-stage pipeline:

```
+-----------------------------------------------------------------------------------+
|                        Deterministic Email Agent Pipeline                         |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Inbound MIME Ingestion & SPF/DKIM Verification]                        |
|     --> Ingest payload from Cloudflare Email Routing & verify domain authenticity  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Mailbox Durable Object Hydration & Storage Persistence]                |
|     --> Route to isolated DO, store body in SQLite, & stream attachments to R2    |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Thread Reconstruction & Urgency Priority Scoring]                      |
|     --> Trace References chain, score sender importance, & extract action items   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Autonomous Draft Composition & Safety Verification Gate]              |
|     --> Generate contextual draft reply via Workers AI & verify policy guardrails |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: User Presentation & Explicit Human Send Confirmation]                  |
|     --> Present draft in UI/MCP; wait for human approval before outbound dispatch |
+-----------------------------------------------------------------------------------+
```

### 2. Decision Logic & Routing Formulations



### 3. Thresholding & Refusal Decision Criteria

# Explainability & Decision Transparency Report enforces strict operational boundaries and deterministic refusal thresholds:
- **Refusal on Policy Violation**: Requests violating boundary constraints halt with code `ERR_POLICY_VIOLATION`.
- **Refusal on Timeout**: Executions exceeding budget limits terminate with code `ERR_EXECUTION_TIMEOUT`.

### 4. Fallback Decision Mechanism

Continuous operational stability is maintained through layered fault recovery:
- **Model Fallback Cascade**: High-level reasoning and synthesis default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Human operators retain sovereign authority over the multi-agent execution lifecycle:
- **Operational Review**: Sensitive actions require operator sign-off.
- **Audit Logging**: All decisions are recorded for auditability.

---

## The Data It Uses

# Explainability & Decision Transparency Report operates under strict principles of data minimization, environment isolation, and privacy protection.

### 1. Ingested Input Data

The framework processes only operational data necessary to perform its functions:
- **Input Directives**: Operational tasks and data payloads.

### 2. Configuration & Reference Data

- **Configuration Schemas**: Declarative system configuration files.

### 3. Base Model & Inference Lineage

- **Language Models**: Cloudflare Workers AI (`@cf/moonshotai/kimi-k2.5`, `@cf/meta/llama-3.3-70b-instruct`).
- **Application Stack**: React 19, Hono, Cloudflare Agents SDK (`AIChatAgent`), TypeScript.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against indirect prompt injection, credential leakage, and unauthorized external API dispatch.
- **Local Environment Isolation**: Agent execution workspaces, intermediate scratchpads, and vector stores reside strictly within designated local project directories.
- **Automated Secret Scrubbing**: API keys, database credentials, and personal credentials are automatically redacted prior to embedding or logging.
- **Zero Commercial Monetization**: Prompts, intermediate reasoning trajectories, and task deliverables are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of # Explainability & Decision Transparency Report is essential for effective deployment.

### 1. Deterministic Multi-Stage Decision Pipeline
The agent manages incoming correspondence, thread reasoning, and reply composition through a deterministic, 5-stage pipeline on Cloudflare Workers.

```
+-----------------------------------------------------------------------------------+
|                        Deterministic Email Agent Pipeline                         |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Inbound MIME Ingestion & SPF/DKIM Verification]                        |
|     --> Ingest payload from Cloudflare Email Routing & verify domain authenticity  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Mailbox Durable Object Hydration & Storage Persistence]                |
|     --> Route to isolated DO, store body in SQLite, & stream attachments to R2    |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Thread Reconstruction & Urgency Priority Scoring]                      |
|     --> Trace References chain, score sender importance, & extract action items   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Autonomous Draft Composition & Safety Verification Gate]              |
|     --> Generate contextual draft reply via Workers AI & verify policy guardrails |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: User Presentation & Explicit Human Send Confirmation]                  |
|     --> Present draft in UI/MCP; wait for human approval before outbound dispatch |
+-----------------------------------------------------------------------------------+
```

### 2. Mathematical Decision & Affinity Scoring
Inbound email urgency scoring and folder routing apply a weighted multi-factor formulation:

$$S_{\text{urgency}}(m) = w_1 \cdot \text{SenderAffinity}(m) + w_2 \cdot \text{TimeSensitivity}(m) + w_3 \cdot \text{ActionRelevance}(m) - w_4 \cdot \text{SpamScore}(m)$$

Where:
- $w_1 = 0.40$: Historical sender interaction frequency and domain whitelist weight.
- $w_2 = 0.25$: Extracted calendar deadlines and temporal indicators (e.g., 'EOD', 'urgent').
- $w_3 = 0.20$: Presence of direct questions or explicit calls-to-action in email body.
- $w_4 = 0.15$: Heuristic spam and newsletter classification penalty.

Semantic search ranking across stored conversation threads is computed as:

$$R(t_i, q) = \mu \cdot \text{CosineSim}(\mathbf{e}_{t_i}, \mathbf{e}_q) + (1 - \mu) \cdot \text{BM25}(t_i, q)$$

Where $\mathbf{e}$ represents Workers AI text embeddings, and $\mu = 0.65$ balances vector semantics with keyword precision.

### 3. Thresholding & Refusal Decision Criteria
When inbound emails or requested agent actions violate security constraints, processing is halted with explicit error codes:

| Threshold Parameter | Value | Decision / Refusal Action | Error Code |
| :--- | :--- | :--- | :--- |
| **Unconfirmed Outbound Send** | Missing human approval token | Block outbound delivery attempt | `ERR_UNCONFIRMED_SEND_BLOCKED` |
| **Invalid Email Address Syntax** | RFC 5322 regex failure | Reject draft creation or delivery | `ERR_INVALID_RECIPIENT_ADDRESS` |
| **Attachment Size Overflow** | $> 25$ MB | Refuse inline attachment; require R2 link | `ERR_ATTACHMENT_SIZE_EXCEEDED` |
| **Cloudflare Access JWT Expired** | Expired / invalid signature | Deny access with 401 Unauthorized | `ERR_ACCESS_TOKEN_INVALID` |
| **Outbound Send Rate Limit** | $> 50$ emails/hour | Throttle outbound email delivery queue | `ERR_SEND_RATE_LIMIT_EXCEEDED` |

### 4. Multi-Tier Fallback Mechanisms & Human-in-the-Loop Governance
1. **Tier 1 (Automated Retry)**: If Workers AI model inference encounters transient rate-limiting, the agent retries with exponential backoff (1s, 2s).
2. **Tier 2 (Draft-Only Safe Mode)**: If any ambiguity exists regarding recipient intent or thread context, the agent defaults strictly to saving an editable draft without prompting for send.
3. **Tier 3 (Mandatory Human Sign-off)**: Outbound message transmission (`send_email`) is hardcoded to require physical user interaction in the React UI or explicit confirmation via MCP parameters.

---

## The Data It Uses

### 1. Ingestion Data & Input Types
- **MIME Email Messages**: Headers (From, To, Cc, Subject, Date, Message-ID), plain text, and HTML bodies.
- **File Attachments**: Documents, PDFs, and images streamable to Cloudflare R2 storage.
- **Search Queries**: Natural language search phrases and folder filter parameters.

### 2. Reference Storage & Database Engines
- **Durable Object SQLite**: Relational tables housing mailbox folders, emails, threads, drafts, and rate-limiting counters.
- **Cloudflare R2**: Object storage for large attachments and raw EML message archives.
- **Workers AI**: On-edge inference for drafting and semantic vector embeddings.

### 3. Model Lineage & System Architecture
- **Language Models**: Cloudflare Workers AI (`@cf/moonshotai/kimi-k2.5`, `@cf/meta/llama-3.3-70b-instruct`).
- **Application Stack**: React 19, Hono, Cloudflare Agents SDK (`AIChatAgent`), TypeScript.

### 4. Data Privacy, Governance & Retention
- **Single-Tenant Mailbox Isolation**: Each mailbox resides in its own isolated Durable Object instance with no cross-tenant query access.
- **Cloudflare Access Zero Trust**: Ingress requires organizational SSO authentication via Cloudflare Access JWT.
- **Configurable Retention**: Deleted emails placed in Trash undergo automatic SQLite purging after 30 days.

---

## Limitations

### 1. Cold Start Ingestion Latency for Large Attachments
- **Limitation**: Processing multi-megabyte PDF attachments during MIME parsing can increase Worker execution time.
- **Mitigation**: Stream attachments directly into Cloudflare R2 without buffering full binary bodies in memory.

### 2. Domain-Level Email Routing Dependency
- **Limitation**: Inbound reception requires active DNS delegation and MX record configuration on Cloudflare.
- **Mitigation**: Provide automated DNS configuration verification and pre-flight validation scripts.

### 3. HTML Rich Text Formatting Nuances
- **Limitation**: Complex legacy email HTML with nested tables may undergo minor style stripping during markdown conversion.
- **Mitigation**: Store raw MIME streams alongside sanitized plain-text representations for high-fidelity viewing.

### 4. Rate Limits on Cloudflare Outbound Email Service
- **Limitation**: Newly configured Cloudflare Email Service accounts operate under initial daily sending volume caps.
- **Mitigation**: Enforce client-side rate limit tracking and queue pending outbound emails in SQLite.

### 5. Multi-Party Email Thread Divergence
- **Limitation**: When multiple recipients reply simultaneously, thread references may branch into parallel conversations.
- **Mitigation**: Construct tree-based conversation DAGs based on message IDs rather than simple linear chains.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested input data & query streams | Section 1 | Verified |
| - Configuration & reference schemas | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Deterministic Multi-Stage Decision Pipeline
The agent manages incoming correspondence, thread reasoning, and reply composition through a deterministic, 5-stage pipeline on Cloudflare Workers.

```
+-----------------------------------------------------------------------------------+
|                        Deterministic Email Agent Pipeline                         |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Inbound MIME Ingestion & SPF/DKIM Verification]                        |
|     --> Ingest payload from Cloudflare Email Routing & verify domain authenticity  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Mailbox Durable Object Hydration & Storage Persistence]                |
|     --> Route to isolated DO, store body in SQLite, & stream attachments to R2    |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Thread Reconstruction & Urgency Priority Scoring]                      |
|     --> Trace References chain, score sender importance, & extract action items   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Autonomous Draft Composition & Safety Verification Gate]              |
|     --> Generate contextual draft reply via Workers AI & verify policy guardrails |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: User Presentation & Explicit Human Send Confirmation]                  |
|     --> Present draft in UI/MCP; wait for human approval before outbound dispatch |
+-----------------------------------------------------------------------------------+
```

### 2. Mathematical Decision & Affinity Scoring
Inbound email urgency scoring and folder routing apply a weighted multi-factor formulation:

$$S_{\text{urgency}}(m) = w_1 \cdot \text{SenderAffinity}(m) + w_2 \cdot \text{TimeSensitivity}(m) + w_3 \cdot \text{ActionRelevance}(m) - w_4 \cdot \text{SpamScore}(m)$$

Where:
- $w_1 = 0.40$: Historical sender interaction frequency and domain whitelist weight.
- $w_2 = 0.25$: Extracted calendar deadlines and temporal indicators (e.g., 'EOD', 'urgent').
- $w_3 = 0.20$: Presence of direct questions or explicit calls-to-action in email body.
- $w_4 = 0.15$: Heuristic spam and newsletter classification penalty.

Semantic search ranking across stored conversation threads is computed as:

$$R(t_i, q) = \mu \cdot \text{CosineSim}(\mathbf{e}_{t_i}, \mathbf{e}_q) + (1 - \mu) \cdot \text{BM25}(t_i, q)$$

Where $\mathbf{e}$ represents Workers AI text embeddings, and $\mu = 0.65$ balances vector semantics with keyword precision.

### 3. Thresholding & Refusal Decision Criteria
When inbound emails or requested agent actions violate security constraints, processing is halted with explicit error codes:

| Threshold Parameter | Value | Decision / Refusal Action | Error Code |
| :--- | :--- | :--- | :--- |
| **Unconfirmed Outbound Send** | Missing human approval token | Block outbound delivery attempt | `ERR_UNCONFIRMED_SEND_BLOCKED` |
| **Invalid Email Address Syntax** | RFC 5322 regex failure | Reject draft creation or delivery | `ERR_INVALID_RECIPIENT_ADDRESS` |
| **Attachment Size Overflow** | $> 25$ MB | Refuse inline attachment; require R2 link | `ERR_ATTACHMENT_SIZE_EXCEEDED` |
| **Cloudflare Access JWT Expired** | Expired / invalid signature | Deny access with 401 Unauthorized | `ERR_ACCESS_TOKEN_INVALID` |
| **Outbound Send Rate Limit** | $> 50$ emails/hour | Throttle outbound email delivery queue | `ERR_SEND_RATE_LIMIT_EXCEEDED` |

### 4. Multi-Tier Fallback Mechanisms & Human-in-the-Loop Governance
1. **Tier 1 (Automated Retry)**: If Workers AI model inference encounters transient rate-limiting, the agent retries with exponential backoff (1s, 2s).
2. **Tier 2 (Draft-Only Safe Mode)**: If any ambiguity exists regarding recipient intent or thread context, the agent defaults strictly to saving an editable draft without prompting for send.
3. **Tier 3 (Mandatory Human Sign-off)**: Outbound message transmission (`send_email`) is hardcoded to require physical user interaction in the React UI or explicit confirmation via MCP parameters.

---

## The Data It Uses

### 1. Ingestion Data & Input Types
- **MIME Email Messages**: Headers (From, To, Cc, Subject, Date, Message-ID), plain text, and HTML bodies.
- **File Attachments**: Documents, PDFs, and images streamable to Cloudflare R2 storage.
- **Search Queries**: Natural language search phrases and folder filter parameters.

### 2. Reference Storage & Database Engines
- **Durable Object SQLite**: Relational tables housing mailbox folders, emails, threads, drafts, and rate-limiting counters.
- **Cloudflare R2**: Object storage for large attachments and raw EML message archives.
- **Workers AI**: On-edge inference for drafting and semantic vector embeddings.

### 3. Model Lineage & System Architecture
- **Language Models**: Cloudflare Workers AI (`@cf/moonshotai/kimi-k2.5`, `@cf/meta/llama-3.3-70b-instruct`).
- **Application Stack**: React 19, Hono, Cloudflare Agents SDK (`AIChatAgent`), TypeScript.

### 4. Data Privacy, Governance & Retention
- **Single-Tenant Mailbox Isolation**: Each mailbox resides in its own isolated Durable Object instance with no cross-tenant query access.
- **Cloudflare Access Zero Trust**: Ingress requires organizational SSO authentication via Cloudflare Access JWT.
- **Configurable Retention**: Deleted emails placed in Trash undergo automatic SQLite purging after 30 days.

---

## Limitations

### 1. Cold Start Ingestion Latency for Large Attachments | Section 1 | Verified |
| - Domain-Level Email Routing Dependency | Section 2 | Verified |
| - HTML Rich Text Formatting Nuances | Section 3 | Verified |
| - Rate Limits on Cloudflare Outbound Email Service | Section 4 | Verified |
| - Multi-Party Email Thread Divergence | Section 5 | Verified |
