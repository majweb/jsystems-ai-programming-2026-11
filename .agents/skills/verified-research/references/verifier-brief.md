# Verifier brief — template

Verifiers must not be the agents that produced the numbers. Group claims by source so one verifier handles 3–8 sources.

```
You are <V-ID>, an independent fact-checker for a report on <topic>. Other agents extracted the numbers below from primary sources using a summarising fetch tool, which can garble table cells. Open each primary source yourself with WebFetch (prefer arxiv.org/html/<id>; no package installs, no downloads, no curl) and check each CLAIMED value cell by cell. Ask targeted prompts like "Return verbatim the header row and the rows for X and Y from Table N". Do not trust the claims below — they are what you are testing.

## Claims
C1. <source id + URL>, <Table/section>: <row> = <value>; <row> = <value> ...
    Also: <open question, e.g. "what is the unit/metric?", "is language X included?", "is this a per-model value or an average?">
C2. ...

## Output
Write to <scratchpad>/research/<V-ID>.md and return the same content:
| Claim | Value claimed | Value found | Match? (YES/NO/PARTIAL/COULD NOT CHECK) | Location | Note |
Then a mismatch list with corrected values and verbatim snippets. Be strict: if you cannot see the exact cell, write COULD NOT CHECK rather than YES.
```

## What to send to verifiers

- Every table that will be charted in the artifact.
- Headline numbers quoted in "Key findings".
- Anomalies (a weaker configuration beating a stronger one, identical values in adjacent cells, numbers far outside the range of their peers).
- Units and metric definitions when unclear (accuracy vs win rate vs ratio of totals).
- Anything you could not fetch yourself because of a denied tool call.
