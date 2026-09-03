# Prevent repeated requests from silently repeating completed effects — tested against 5 cases

Evidence package for Operator Required episode `OR-3A2CC1D9`.

- Publication package: `pub-705c782d4b51475a80ab1502a1f31513`
- Accepted master SHA-256: `bcb55b74f7f704ed1c58f7e4a9a1626af16d9d40283351db5b16193d48fedb51`
- Contract hash: `52cc4b47c4a8e1b0aec6a3b19a6d8842faa9673eff52a9e8f9a07ce78b3c6de1`
- Implementation hash: `8126374a219d09c3147a86789b6e1e10dcf396e3ef59026e44f415038a8e5b65`

## The problem

`extensions/notes/filing.go` builds a `CreateActivityRequest` with no `source_system`/`source_id`, so `activities.replayedActivity` never dedupes it. `file_note` is an `auto_execute` 🟢 tool, so a client or network retry writes two activities and two notes.

## The rule under test

Prevent repeated requests from silently repeating completed effects.

## Recorded cases

| Case | Expected | Actual | Passed |
| --- | --- | --- | --- |
| NEW_OPERATION | `{"classification": "NEW_OPERATION", "decision": "ACCEPT"}` | `{"classification": "NEW_OPERATION", "decision": "ACCEPT"}` | yes |
| COMPLETED_REPEAT | `{"classification": "ALREADY_COMPLETED", "decision": "NOOP"}` | `{"classification": "ALREADY_COMPLETED", "decision": "NOOP"}` | yes |
| FAILED_RETRY | `{"classification": "SAFE_RETRY", "decision": "RETRY"}` | `{"classification": "SAFE_RETRY", "decision": "RETRY"}` | yes |
| CHANGED_PAYLOAD | `{"classification": "PAYLOAD_CONFLICT", "decision": "OPERATOR_REQUIRED"}` | `{"classification": "PAYLOAD_CONFLICT", "decision": "OPERATOR_REQUIRED"}` | yes |
| MISSING_ID | `{"classification": "MISSING_OPERATION_ID", "decision": "BLOCK"}` | `{"classification": "MISSING_OPERATION_ID", "decision": "BLOCK"}` | yes |

Each case in `fixtures.json` carries the SHA-256 of its own recorded evidence.

## Boundary

These runs were executed locally against fixed inputs, with no credentials, no network use during testing, and no external writes. They are not a claim about any third-party project's own code.

An Mt. Wizard Studios operation.
