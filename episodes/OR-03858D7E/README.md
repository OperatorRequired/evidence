# Accept known complete headers, block broken structure, and hold unfamiliar meaning for an operator

Evidence package for Operator Required episode `OR-03858D7E`.

- Publication package: `pub-a7b91e0a5bd2463291e242a7bdc4c73f`
- Accepted master SHA-256: `0648e671da0423f8fa1e425a5a06429d14dc20feb123b7565aa3403ce16c2692`
- Contract hash: `3bf919acf984d3400d77ca9593d2df0d9cab803c4f8ddfc4a3fb53dc8c1e2bfa`
- Implementation hash: `e36804808c9ede3431f183edd2b30e26a24dfa3768e547d5ad289059036c3306`

## The problem

The CSV header does not match the content of the table when exporting hosts DB for protocols other than RDP. It's due of the use of the same headers in the file nxc/nxcdb.py for all protocols, whereas it is specific to SMB.

## The rule under test

Accept known complete headers, block broken structure, and hold unfamiliar meaning for an operator.

## Recorded cases

| Case | Expected | Actual | Passed |
| --- | --- | --- | --- |
| EXPECTED | `{"classification": "EXPECTED", "decision": "ACCEPT"}` | `{"classification": "EXPECTED", "decision": "ACCEPT"}` | yes |
| REORDERED | `{"classification": "REORDERED", "decision": "ACCEPT"}` | `{"classification": "REORDERED", "decision": "ACCEPT"}` | yes |
| MISSING_REQUIRED | `{"classification": "MISSING_REQUIRED", "decision": "BLOCK"}` | `{"classification": "MISSING_REQUIRED", "decision": "BLOCK"}` | yes |
| UNKNOWN_HEADER | `{"classification": "UNKNOWN_HEADER", "decision": "OPERATOR_REQUIRED"}` | `{"classification": "UNKNOWN_HEADER", "decision": "OPERATOR_REQUIRED"}` | yes |
| DUPLICATE_HEADER | `{"classification": "DUPLICATE_HEADER", "decision": "BLOCK"}` | `{"classification": "DUPLICATE_HEADER", "decision": "BLOCK"}` | yes |

Each case in `fixtures.json` carries the SHA-256 of its own recorded evidence.

## Boundary

These runs were executed locally against fixed inputs, with no credentials, no network use during testing, and no external writes. They are not a claim about any third-party project's own code.

An Mt. Wizard Studios operation.
