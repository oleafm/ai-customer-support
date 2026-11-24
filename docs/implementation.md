# Implementation plan

**Workflow status: Planned.** This is a build plan, not setup instructions for a runnable integration. No API connections, workflow exports or deployment have been completed.

## Suggested build sequence

1. Choose one intake channel, a sandbox CRM and a durable event store. Define verified identity and tenant mapping before customer lookup.
2. Implement validation, a persistent event claim and the duplicate path before calling external services.
3. Define the classification schema and a versioned prompt. Start with synthetic billing, technical, access and product requests.
4. Add a CRM lookup restricted to the verified customer scope. Keep lookup failures distinct from a confirmed missing record.
5. Start documentation retrieval with an approved corpus and source IDs. Evaluate RAG separately before adding it to routing.
6. Implement explicit routing rules and an authenticated review queue with approve, reject and expiry states.
7. Add Slack delivery, persisted action state and failure recovery. Replay safely from the last incomplete stage.
8. Execute the test plan, export the real Make blueprint and replace proposed claims only after checking the running scenario.

## Prerequisites to choose or configure

- **CRM and customer identity:** Select a sandbox CRM and prove the source-to-customer mapping; never trust an arbitrary submitted customer ID.
- **Persistence and concurrency:** Choose a store with atomic event claims; confirm whether Make storage meets the required concurrency semantics.
- **RAG scope:** Choose corpus ownership, retrieval permissions and evidence requirements; a vector database is not yet selected.
- **Human approval:** Choose the review interface, reviewer roles, expiry policy and audit record.

Keep credentials in the selected platform's credential store. Use synthetic records and sandbox accounts when building the showcase. No secret values belong in this repository.

## Implementation record — placeholder

| Item | Current record |
| --- | --- |
| Workflow/export version | Not implemented |
| Platform and tool versions | To be recorded after selection |
| Configured API connections | None documented |
| Running environment | None documented |
| Verified scope | None |
| Known gaps | All planned capabilities remain unimplemented |
| Results | Not measured |

Replace these rows with actual versions and evidence as work progresses. Keep the [README checklist](../README.md) and [architecture](architecture.md) consistent. Follow the [maintenance guide](maintenance.md) when adding real artifacts.
