# Proposed security and reliability controls

**Workflow status: Planned.** The controls below are design requirements to implement and test, not verified protections.

## Project-specific trust boundaries

- Treat email bodies, webhook fields and retrieved text as untrusted data. Keep them separate from trusted instructions; do not allow model output to choose API URLs, tools or credentials.
- Derive tenant and customer access from verified identity. An email address alone is not sufficient authority for account disclosure or recovery.
- Retrieve only approved documentation and authorised CRM fields. Preserve source IDs and route missing evidence to a person.
- Request only the minimum customer context needed for classification. Do not send payment details, credentials or full CRM profiles to the model.
- Use an authenticated approval record tied to the case and proposed action. A Slack message or LLM response must not count as approval.
- Prompt injection protection is a set of boundaries and tests, not a claim that filtering makes untrusted text safe.

## Authentication and secrets

Verify webhook signatures and timestamps when supported, including replay handling. If the source lacks signing, choose an authenticated gateway or another provider-supported verification method before exposing intake. Store credentials in the orchestrator's credential store, use minimum permissions and rotate or revoke them when needed.

Keep tokens, OAuth refresh material, connection identifiers, private webhook URLs and service credentials out of exports, prompts, screenshots and logs. Use sandbox connections for demonstrations. A public repository must contain only reviewed documentation and sanitised artifacts.

## Human review and authorisation

Bind reviewer decisions to an authenticated actor, the exact record revision, proposed action and expiry time. Reject stale or replayed decisions and record approval/rejection reasons. Notifications are pointers to the review process; a message, extracted phrase or model flag alone does not authorise action.

## Data minimisation and retention

Define what data each provider receives before configuration. Review provider retention and processing settings against the intended use. Select retention periods for source inputs, model payloads, execution logs and review records; these periods are not yet decided. Restrict access and document deletion handling before processing any real data.

Only clearly labelled synthetic inputs may be committed. Do not include real customer, lead, supplier or invoice information even when demonstrating a failure case.

## Reliability and publication review

Implement the [durable execution and retry design](architecture.md). Test authorisation, duplicate delivery, partial failure and injection attempts using the [test matrix](testing.md). Record the observed limits instead of claiming complete protection.

The [maintenance guide](maintenance.md) describes artifact sanitisation. `.gitignore` is a convenience, not a security control; inspect tracked files and image contents before publishing.
