# Testing and evaluation plan

**Workflow status: Planned. Tests not executed.** Expected outcomes below are specifications, not test results. No fixture files, benchmark scores or runtime evidence exist yet.

## Test environment and data

Use sandbox connections, synthetic records, a controlled model configuration and isolated notification destinations. Stub external failures first, then verify behaviour against actual sandbox APIs. Record provider/tool versions, prompt version, rule/schema versions and export revision.

Create each case with a stable case ID, labelled input, expected decision, expected external side effects and expected state transition. Keep [synthetic fixtures](../sample-data/README.md) separate from measured outputs. Repeat delivery and concurrency tests with overlapping runs.

## Planned test matrix

| Case | Synthetic input or failure | Expected outcome |
| --- | --- | --- |
| S01 | Valid synthetic technical request | Schema-valid classification and the expected queue; one case and one notification |
| S02 | Repeated event and concurrent deliveries | One event claim; no repeated side effects; retry resumes incomplete work |
| S03 | Refund or account-recovery request | Human review regardless of a routine model label |
| S04 | Instruction injection in request or retrieved text | No tool selection, secret disclosure or unauthorised action |
| S05 | LLM refusal, timeout or invalid JSON | Recorded failure or review state; no automatic sensitive action |
| S06 | Cross-tenant customer reference or missing documentation | Access denied or evidence gap held for review |
| S07 | CRM rate limit and Slack failure after case creation | Bounded recovery without recreating the case |
| S08 | Expired, rejected or replayed approval | No execution from an invalid approval |

## Evaluation method

Report intent and routing agreement against human-labelled synthetic cases, escalation behaviour, duplicate side effects, latency and per-run API usage. Record dataset size, prompt/model version and failure cases. Define acceptance thresholds before running the evaluation; no accuracy, cost or speed figures are available.

Critical safety cases must show no unauthorised action; replay tests must show no duplicate side effects. These are acceptance requirements, not passed assertions. Version the labelled set before testing and retain an evaluation subset when tuning prompts.

## Results — placeholder

| Evidence | Current state |
| --- | --- |
| Synthetic dataset and expected outputs | Not prepared |
| Test execution date/environment | Not available |
| Cases run / pass / fail | Not measured |
| Extraction or classification quality | Not measured |
| Reliability and recovery evidence | Not measured |
| Latency and API usage | Not measured |
| Known limitations | Implementation and evaluation still pending |

After execution, add a per-case result with run date, workflow/export revision, expected/actual behaviour and sanitised evidence. Report failures and dataset limitations alongside successes. Do not mark the project Tested until the documented tests have actually run.

See [implementation](implementation.md) and [maintenance](maintenance.md).
