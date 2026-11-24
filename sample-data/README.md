# Synthetic sample data

**Placeholder — no synthetic dataset has been prepared yet.**

Organise future fixtures by billing, technical, account access, product questions, duplicate events and adversarial inputs. Use invented customer identifiers and a synthetic documentation corpus with explicit source labels.

When fixtures exist, organise them into `inputs/`, `expected/` and `results/`, with matching stable case IDs. Add an index describing each case, its provenance as synthetic data, expected decisions and side effects, and the schema/rule version used. Mark measured outputs with the workflow revision and execution date; expected outputs are not test results.

Never use real customer data, copied production payloads, secrets or private identifiers. Synthetic files must be clearly labelled in both filenames/metadata and the dataset index.

See the [test matrix](../docs/testing.md) and [maintenance guide](../docs/maintenance.md).
