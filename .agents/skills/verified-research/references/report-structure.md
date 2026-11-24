# Report structure (Markdown, one file per language)

Write the English version first, then a faithful translation (numbers formatted for the locale, e.g. `79,7%` in Polish). Each file links to the other.

```
# <Topic title>

<One line: "Evidence report, compiled <date>." + link to the other language.>

## Scope and method
- Question
- Rules (primary sources only; what secondary sources were used for)
- Process: N research agents (model) with scopes A1…An, iterations per agent, native-language searches, M verifiers re-checking K tables, raw reports in agent-reports/
- Status labels: V / P / SELF
- Limitations (e.g. summarising fetch tool, PDFs not parsed locally)

## Key findings
5–8 numbered findings, each a bold one-sentence claim + the 2–3 numbers that support it.
Include at least one finding that corrects a popular misconception, if the data shows one.

## 1…n. Thematic sections
Each table: caption with source id, table number and status label, e.g.
"### 1.1 <Name> (arXiv 2412.00559v2, Table 4) — V"
Then 1–3 sentences of interpretation and the authors' own caveats.

## Gaps and caveats
What could not be found or verified, self-reporting bias, age of evaluated systems.

## Corrections made during verification
Bulleted list: wrong claim → what the primary source actually says.

## Sources (primary)
Table: ID | what it is | link. (The second-language file may link to this section instead of repeating it.)
```

Style: short direct sentences; tables over prose for numbers; no claim without a source id; never round away a sign (−0.04 is not "equal").
