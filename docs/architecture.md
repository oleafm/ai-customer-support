# Proposed architecture

**Workflow status: Planned.** No services, database or connections have been provisioned for this showcase.

## Scope and boundaries

Use Make.com to validate and deduplicate incoming requests, ask an LLM for structured classification, retrieve authorised customer and documentation context, and apply explicit routing rules. Sensitive or uncertain cases would wait for human review.

The workflow would triage and route support cases. Refunds, subscription changes, account recovery and disclosure of sensitive information would require an authorised human and a separately defined process. Automated customer replies are outside the initial scope.

The [README diagram](../README.md) and [Mermaid source](../diagrams/architecture.mmd) describe the same proposed flow. The orchestrator would coordinate calls; it would not treat model output as execution authority.

## Proposed data contracts

| Contract | Proposed fields and constraints |
| --- | --- |
| Incoming request | `schema_version`, `event_id`, `source`, `received_at`, trusted tenant context, verified customer reference if available, bounded subject/body |
| Classification | `intent`: billing/technical/account_access/product/unknown; `priority`: low/normal/high/urgent; `sentiment`: negative/neutral/positive/unknown; bounded `summary`; `needs_documentation`; `review_reasons` |
| Routing decision | `case_id`, rule version, selected queue, review requirement, reason code |
| Execution record | Correlation ID, event key, stage, state, attempt count, timestamps, redacted error category |

All fields and enums are design proposals. The final JSON schema is not yet implemented. Reject unexpected fields, wrong types and oversized output. An LLM refusal, timeout or malformed result would go to review; it would never imply permission to act.

## Decision rules

| Condition | Proposed deterministic action |
| --- | --- |
| Billing change, account recovery or sensitive data request | Require human review regardless of the model recommendation |
| Unknown intent, missing customer context or invalid model output | Hold for triage |
| Routine technical or product question with authorised context | Create a case in the mapped queue |
| Missing or weak documentation evidence | Record the gap and route for review; do not invent an answer |

Safety rules would inspect explicit request flags and trusted account state as well as validated model fields. Routing priorities and escalation rules must be agreed before implementation. A model's self-reported confidence would not be treated as a calibrated safety guarantee.

## Proposed recovery and observability

Use a durable execution record with `received`, `processing`, `awaiting_review`, `completed`, `rejected` and `failed` states. Claim a stable source/event key atomically; a read-then-write duplicate check alone is insufficient under concurrency. Define leases and recovery for abandoned processing before implementation.

Persist per-action state and external resource IDs. For a timeout after an external write, reconcile provider state before retrying; do not assume a timeout means the write failed. Use provider idempotency keys where available. Exactly-once delivery across independent APIs is not assumed.

Retry transient failures such as rate limits and network/server errors with bounded backoff and jitter, respecting provider retry guidance. Invalid data, authentication failures and unauthorised actions require correction or review. After the retry limit, retain a failure record for deliberate replay from the failed stage.

Log correlation ID, stage, attempt, duration, outcome, rule/prompt version and a redacted error category. Keep payload bodies, credentials and personal information out of routine logs. A status must not become completed if required persistence fails. Surface persistence/claim failures operationally rather than continuing without duplicate protection.

These are proposed requirements; no recovery mechanism, dashboard or operational guarantee is implemented yet.

## Decisions still open

- **CRM and customer identity:** Select a sandbox CRM and prove the source-to-customer mapping; never trust an arbitrary submitted customer ID.
- **Persistence and concurrency:** Choose a store with atomic event claims; confirm whether Make storage meets the required concurrency semantics.
- **RAG scope:** Choose corpus ownership, retrieval permissions and evidence requirements; a vector database is not yet selected.
- **Human approval:** Choose the review interface, reviewer roles, expiry policy and audit record.

See the [implementation plan](implementation.md), [security design](security.md) and [test matrix](testing.md).
