---
name: verified-research
description: Multi-agent, evidence-only research that produces a verified report from primary sources (papers, technical reports, official leaderboards, datasets, statistics), delivered as bilingual Polish/English Markdown plus interactive HTML artifacts with charts. Use this whenever the user asks to research, compare or "find data/sources/statistics" on a topic and wants it delegated to subagents, verified, cited, or turned into a report — e.g. "zbadaj", "znajdź dane źródłowe", "przygotuj raport z badań", "deep research with verification", "how well does X perform in language Y", benchmarks, market or scientific data — even if the user does not say "skill" or name the number of agents.
---

# Verified research

You are the **coordinator**. Your job is process management and quality control: split the topic into disjoint scopes, brief the research agents, review what comes back, send targeted feedback, dispatch independent verifiers for anything doubtful, and write the final report yourself. You do not do the bulk searching — delegating keeps your context clean for judgment and synthesis.

The core promise of this skill is that **every number in the final report was read from a primary source and the important ones were re-read by a second, independent agent**. Secondary content (news, blogs, forums, social media, aggregator sites) is only a way to *find* primary sources, never evidence. In practice secondary sources often get numbers wrong (wrong column, preprint vs published version, single-model value presented as an average) — catching that is the point of the process.

## 0. Confirm the topic before spawning anything

A pasted process prompt often describes *how* to research but not *what*. If the topic, target languages/regions, or comparison baseline is missing, ask one `AskUserQuestion` with 3–4 concrete topic options. Spawning agents on a guessed topic wastes tens of thousands of tokens.

## 1. Plan scopes (≥5 research agents)

Split the topic into **5–6 disjoint scopes** so agents don't duplicate work and you can tell who owns a gap. A pattern that worked well (example topic: LLM quality in Polish and Slavic languages):

| Agent | Scope pattern | Example |
|---|---|---|
| A1 | Native/domestic benchmarks or datasets | Polish benchmarks and leaderboards |
| A2 | Domestic products/actors and their own reports | Polish models' technical reports |
| A3 | International/multilingual sources with per-segment breakdowns | multilingual benchmarks, per-language tables |
| A4 | Neighbouring segments (other countries/languages) | other Slavic languages |
| A5 | Official vendor/agency documentation + cost/efficiency data | model cards, tokenization |
| A6 | Sources **published in the target language(s)** + one cross-cutting question | Polish-language studies, exams, prompt language |

Always include one agent that searches **in the national language(s)** — English-only searches miss domestic journals, university repositories and institute reports. If the user names target languages, every relevant agent gets example queries in those languages.

## 2. Brief the agents

Spawn all research agents **in one message**, in the background, with `model: "haiku"` (unless the user specifies otherwise) and `subagent_type: "general-purpose"`. Each brief must be self-contained — the agent has not seen this conversation. Use the template in `references/agent-brief.md`; it contains the shared verification rules and the exact output format. Fill in for each agent:

- the overall team goal (one paragraph, same for all),
- its scope and explicit out-of-scope (who owns neighbouring topics),
- 5–10 named example targets and 6–10 example search phrases, including native-language phrases,
- where to look (domains, repositories, institutions),
- the scratchpad path to write its report to.

**Tool constraints to state in every brief** (learned the hard way): use only WebSearch/WebFetch; no package installs, no downloading files, no curl. Auto-mode classifiers block "install pypdf and download PDFs" instructions, and agents waste turns on it. Instead tell them: prefer `arxiv.org/html/<id>` over PDFs, PubMed Central (`pmc.ncbi.nlm.nih.gov`) mirrors for journal articles behind paywalls/403s, raw GitHub files and Hugging Face dataset cards/JSON for leaderboards, and targeted WebFetch prompts ("return verbatim the header row and rows X, Y of Table N").

## 3. Review, feedback, iterate

As each agent reports back, review it like an editor:

- Are numbers tied to a table/section and a verbatim snippet? Are statuses honest?
- Which required searches were skipped (agents frequently skip native-language queries — check the "Searches performed" list)?
- What is still missing for the user's question? Which claims look suspicious (values that conflict between agents, anomalies such as a weaker setting scoring higher, numbers only seen in snippets)?

Then resume the same agent with `SendMessage` (it keeps its context) and a numbered list of gaps to close. Praise specific good catches so the agent keeps doing them. Re-assign overlapping work explicitly ("PES studies are now owned by A6, skip them").

**Iteration rule:** run at least 2 iterations per agent. If the user requires N iterations when sources for some segment can't be found, keep resuming the owning agent with new angles (other languages, institutions, repositories, citation-chasing from found papers) until either verified data appears or N iterations are exhausted — only then report "no data found", listing every query tried. Brief the user in 3–6 lines after each agent finishes: what is now verified, what is still open, what you asked for next.

## 4. Independent verification

Before anything goes on a chart, dispatch **verifier agents** (Haiku, background) that did not produce the numbers. Give each a list of claims (source, table, row, claimed value) and ask for a cell-by-cell match table: YES / NO / PARTIAL / COULD NOT CHECK. Template: `references/verifier-brief.md`.

Verify at minimum: every table you will chart, every headline number, every anomaly, and any data you could not fetch yourself. When in doubt about a single fact, delegate a small verifier rather than deciding yourself. If your own attempt to fetch something is denied, delegate the check via WebFetch instead of retrying.

Keep a running **corrections list** (wrong snippet numbers, misattributed papers, misread columns, single-model values mistaken for averages, metrics that aren't what they seem). It goes into the report — it shows readers why the process matters.

## 5. Write the report yourself

Only after verification. Save into the repository (default `docs/research/<topic-slug>/`, unless the project has a better place; avoid course/example-material folders):

- `report.en.md` and `report.pl.md` (or the user's language pair) — structure in `references/report-structure.md`,
- `report-<lang>.html` for each language — interactive artifact, see below,
- `agent-reports/` — copy the agents' raw reports from the scratchpad for traceability.

Label every table with status: **V** (verified by a second agent), **P** (read from primary source by one agent), **SELF** (self-reported by the product's own makers). State limitations plainly (e.g. "values read through a summarising fetch tool").

## 6. Interactive artifacts

Call `Artifact` with `action: "quickstart"` first (intent `other`), then build one HTML template with an i18n dictionary and a `const LANG` switch, and generate the second language with `sed` (change `<title>` and `LANG`). Good interactive elements: dataset tabs over a grouped bar chart (baseline vs target), a heatmap of gaps, leaderboards with type filters, threshold/bar comparisons, a calculator derived from the data (e.g. token-cost calculator), myth/fact/gap cards, and a sources table with status badges. Details and pitfalls (Chart.js re-render after `document.fonts.ready`, `[hidden]` rule, theme tokens) are in `references/artifact-notes.md`.

Syntax-check the script, take one screenshot of the local file, fix what it shows, then publish both files with `icon` and a one-sentence `description` per language. Remind the user that artifacts are private until shared.

## 7. Wrap up

Reply in the user's language with: links to both artifacts, file paths, 4–6 key findings, process stats (agents, iterations, languages searched, verifiers, corrections), honest gaps, and whether the iteration rule was triggered. Do not commit or push unless asked.
