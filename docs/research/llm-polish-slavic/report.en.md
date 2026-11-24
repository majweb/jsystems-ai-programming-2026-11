# LLM Quality in Polish and Other Slavic Languages

Evidence report, compiled 2026-11-24. Language: English. Polish version: [report.pl.md](report.pl.md).

## Scope and method

**Question.** How well do large language models (LLMs) perform in Polish and other Slavic languages, compared with English?

**Rules.** Only primary sources are cited: benchmark papers, technical reports, official leaderboards, dataset cards and peer-reviewed studies. Press articles, blogs and social media were used only to find those sources. Every number below was read from the primary source.

**Process.**

- **Research:** 6 research agents (Claude Haiku) with separate scopes:
  - A1: Polish-native benchmarks
  - A2: Polish models and their technical reports
  - A3: multilingual benchmarks
  - A4: other Slavic languages
  - A5: vendor model cards and tokenization
  - A6: sources published in Polish, exam studies, and the effect of the prompt language
- **Iterations:** A1, A2, A3, A5 and A6 each ran 2 iterations; A4 ran 3. Each new iteration responded to coordinator feedback.
- **Native-language searches:** A4 ran searches in Czech, Slovak, Ukrainian, Russian, Slovenian, Croatian/Serbian/Bosnian and Bulgarian. A6 ran searches in Polish.
- **Verification:** 3 independent verifier agents (V1–V3) re-read 14 key tables cell by cell. No cell-level mismatches were found. Corrections are listed in [Corrections made during verification](#corrections-made-during-verification).
- **Raw reports:** in [agent-reports/](agent-reports/).

**Status labels used below.**

- **V**: verified by a second, independent agent.
- **P**: read from the primary source by one agent.
- **SELF**: reported by the model's own developers.

**Limitation.** All values were read through a summarising web-fetch tool. PDFs could not be parsed locally. Numbers that the verifiers re-read agree across two or more readings.

## Key findings

1. **Frontier models are close to parity in Polish on knowledge tasks. Smaller and older models are not.**
   - On the Polish medical final exam (LEK), GPT-4o scores 89.96% in Polish and 88.69% in English.
   - On the same exam, Qwen2.5-7B scores 51.92% in Polish and 67.73% in English, a gap of −15.8 percentage points (pp).
   - On Belebele reading comprehension, every evaluated 2023-era model scores 7.5–17.3 pp lower in Polish than in English.
2. **The gap shrinks as models improve.** On the LEK exam, GPT-3.5 scores 54.8% in Polish and 60.3% in English. GPT-4 scores 79.7% in both languages (Sci Rep 2023).
3. **Commercial models lead on Polish language and culture.** On the Polish Linguistic and Cultural Competency benchmark (PLCC):
   - Gemini-Exp-1206: 83.0%; Claude 3.5 Sonnet: 82.7%; GPT-4o: 81.3%.
   - Best Polish model, Bielik 2.3 (11B): 62.2%.
   - Llama-3.1-405B: 60.0%.
4. **Polish open models compete in their size class, not with frontier models.**
   - LLMzSzŁ benchmark of Polish national exams: Qwen2.5-72B 69.1, Llama-PLLuM-70B 64.4, Bielik-11B-v2.6 59.2.
   - No closed model appears on that leaderboard.
5. **Polish text costs about twice as many tokens as English text.**
   - On the FLORES-200 parallel corpus, GPT-4's tokenizer (cl100k) uses 1.91× as many tokens for Polish as for English.
   - Czech 2.11×, Russian 2.49×, Ukrainian 3.00×.
   - A Polish-specific tokenizer (APT4) lowers Polish from 3.22 to 1.62 tokens per word (SELF, one sample text).
6. **"Polish is the best language for AI" overstates one result.**
   - On OneRuler long-context retrieval (needle-in-a-haystack, NIAH), Polish ranks 1st of 26 languages at 88%. English ranks 6th at 83.9%.
   - This covers synthetic retrieval tasks only and does not generalise to overall prompting quality.
7. **Vendor documentation rarely reports Polish.**
   - These per-language tables contain no Polish row: OpenAI MMMLU (14 languages), Anthropic's Global-MMLU table (14 languages), the Llama 3.1 model card (8 languages) and the GPT-4o tokenizer table (20 languages).
   - MMLU-ProX (29 languages) and Global-MMLU-Lite also exclude Polish.
8. **Other Slavic languages show the same pattern.**
   - Bulgarian (BgGPT): open models lose 10–22 pp compared with English.
   - Slovak (sk-bench, 55 models): no open-weights model reaches the proprietary APIs.
   - MMLU-ProX: frontier reasoning models are within 0.2–3.7 pp of English in Czech, Serbian and Ukrainian.

## 1. Polish vs English on identical questions

### 1.1 Medical exams, parallel PL/EN versions (arXiv 2412.00559v2, Table 4) — V

Same questions in both languages; % correct.

| Model | LEK PL | LEK EN | Gap (pp) | LDEK PL | LDEK EN |
|---|---|---|---|---|---|
| gpt-4o-2024-08-06 | 89.96 | 88.69 | +1.27 | 73.89 | 72.51 |
| Llama-3.1-70B-Instruct | 80.94 | 82.60 | −1.66 | 61.75 | 63.17 |
| Qwen2.5-72B-Instruct (P) | 76.87 | 81.93 | −5.06 | 58.35 | 63.33 |
| Mistral-Large-Instruct-2407 | 76.75 | 82.40 | −5.65 | 56.29 | 62.14 |
| gpt-4o-mini-2024-07-18 | 75.88 | 75.92 | −0.04 | 54.94 | 56.88 |
| Bielik-11B-v2.2-Instruct | 62.36 | 56.98 | +5.38 | 43.20 | 42.88 |
| Qwen2.5-7B-Instruct | 51.92 | 67.73 | −15.81 | 43.71 | 48.38 |
| Llama-3.2-3B-Instruct (P) | 39.22 | 52.08 | −12.86 | 32.16 | 36.87 |

The authors summarise: "models perform significantly better on English questions". GPT-4o and Bielik are the exceptions. The size of the Table 4 subset is not stated.

### 1.2 LEK 2022–2023, GPT-3.5 vs GPT-4 (Rosoł et al., Sci Rep 13:20512, 2023) — V

| Model | Polish | English | Pass mark | Medical students |
|---|---|---|---|---|
| GPT-4 | 79.7% | 79.7% | 56% | 83.0–84.8% |
| GPT-3.5 | 54.8% | 60.3% | 56% | — |

Source: https://pmc.ncbi.nlm.nih.gov/articles/PMC10665355. A frequently cited "80.7%" figure is not in the published paper.

### 1.3 Belebele reading comprehension (arXiv 2308.16884, ACL 2024, Table 7) — V for the first three rows

| Model | EN | PL | Gap | CS | RU | UK | SK | BG | HR | SL | SR |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Llama 2 70B, 5-shot | 90.9 | 79.2 | −11.7 | 81.1 | 82.7 | 80.8 | 75.2 | 80.4 | 79.8 | 76.7 | 81.0 |
| GPT-3.5-turbo, 0-shot | 87.7 | 74.7 | −13.0 | 76.9 | 78.4 | 72.8 | 77.3 | 76.0 | 78.4 | 77.4 | 74.8 |
| Llama-2-chat, 0-shot | 78.8 | 61.7 | −17.1 | 65.0 | 67.0 | 65.7 | 58.8 | 65.0 | 64.7 | 62.4 | 65.1 |
| Llama 1, 5-shot (P) | 82.5 | 67.0 | −15.5 | 70.7 | 73.1 | 69.7 | 60.4 | 69.3 | 66.9 | 65.6 | 70.7 |
| Falcon, 5-shot (P) | 77.2 | 59.9 | −17.3 | 65.0 | 48.1 | 41.9 | 57.0 | 41.9 | 48.7 | 43.7 | 40.2 |
| InfoXLM-L, translate-train (P) | 81.2 | 73.7 | −7.5 | 76.2 | 76.8 | 74.9 | 76.7 | 75.3 | 75.3 | 75.4 | 76.1 |

### 1.4 EU20-MMLU, DeepL machine translation (arXiv 2410.08928v2, Table 1, Llama-3.1-70B-Instruct) — V

| EN | PL | CS | SL | SK | BG |
|---|---|---|---|---|---|
| .82 | .78 | .78 | .74 | .68 | .67 |

The authors caution that "Translation quality likely plays a role in these results".

### 1.5 OneRuler long context (arXiv 2503.01996v2, COLM 2025) — V

- **Across all models (64K–128K context):** Polish ranks 1st of 26 languages with 88% NIAH accuracy. English ranks 6th with 83.9%.
- **One model, o3-mini-high at 128K:** English 67%, Polish 92%, Ukrainian 89%.
- **Not published:** per-language values for other languages appear only in figures.
- **Caveats:**
  - These are synthetic retrieval tasks.
  - One annotator translated both the Polish and the Japanese data.
  - The Polish filler text is a 1934 novel.

## 2. Polish benchmarks: leaderboards

### 2.1 PLCC: Polish linguistic and cultural competency (arXiv 2503.00995, OPI-PIB) — V

600 questions in 6 categories; total %.

| Model | Total | Type |
|---|---|---|
| Gemini-Exp-1206 | 83.00 | commercial |
| Claude-3.5-Sonnet | 82.67 | commercial |
| GPT-4o | 81.33 | commercial |
| Gemini-2.0-Flash (P) | 74.17 | commercial |
| Claude-3-Opus (P) | 73.83 | commercial |
| DeepSeek-V3 | 69.17 | open |
| GPT-4-turbo | 67.00 | commercial |
| Bielik-2.3 (11B) | 62.17 | Polish |
| Llama-3.1-405B | 60.00 | open |
| Mistral-Large-2411 (P) | 52.00 | open |
| Llama-3.3-70B (P) | 48.83 | open |
| Bielik-0.1 (7B) (P) | 46.67 | Polish |
| GPT-3.5-turbo | 43.33 | commercial |
| Qwen-2.5-72B (P) | 39.17 | open |
| Llama-3.1-8B | 22.67 | open |

Grammar is the weakest category even for GPT-4o: 67, against 80–89 in the other categories.

### 2.2 LLMzSzŁ: Polish national exams, ~19k questions (leaderboard JSON and arXiv 2501.02266) — P

Open-weights models only.

| Model | Accuracy |
|---|---|
| Qwen2.5-72B-Instruct | 69.06 |
| Mistral-Large-Instruct-2407 | 67.17 |
| Llama-3.3-70B-Instruct | 67.13 |
| Llama-3.1-70B-Instruct | 66.59 |
| Llama-PLLuM-70B-chat | 64.42 |
| PLLuM-8x7B-nc-chat | 60.52 |
| Bielik-11B-v2.6-Instruct | 59.23 |
| Bielik-11B-v2.1-Instruct | 57.52 |
| trurl-2-13b | 40.22 |
| Qra-13b | 34.85 |

### 2.3 Other Polish benchmarks

| Benchmark | Results | Source |
|---|---|---|
| Open PL LLM Leaderboard (5-shot, instruct) | Qwen2.5-72B 67.92; Llama-3.3-70B 66.40; Bielik-11B-v2.3 65.71; Qwen2.5-14B 59.91 | arXiv 2505.02410v2, Table 8 (SELF, V) |
| Polish EQ-Bench | gpt-4o 75.15; gpt-4-turbo 74.59; Bielik-11B-v3 71.20; gpt-4o-mini 71.15 | arXiv 2604.10799, Table 3 (SELF, P) |
| CPTUB (complex text understanding, about 0–5) | gemini-2.0-flash 4.29; Bielik-PL-11B-v3 3.80; Bielik-11B-v3 3.73 | arXiv 2604.10799, Table 4 (SELF, P) |
| Polish MT-Bench (0–10, GPT-4 judge) | Qwen2.5-32B 8.86; Mistral-Small-2501 8.72; Bielik-11B-v2.3 8.56 | arXiv 2505.02410v2, Table 10 (SELF, P) |
| Polish logic puzzles (111 items) | Gemini-3-pro 87; o3 87; gemini-2.5-pro 84; qwen3:8b 68; bielik-r 56 | arXiv 2603.10640, Table 2 (P) |
| RAG-IFEval (public administration) | Grok-3 91.1; GPT-4.1 90.2; Llama-PLLuM-70B 89.7; GPT-4o 80.3 | arXiv 2511.03823, Table 33 (SELF; written by the PLLuM authors) |

## 3. Polish medical specialty exams (PES) and LEK studies — P

| Study | Model | Score | Pass mark |
|---|---|---|---|
| LEK spring 2021 (Cureus 2024, 196 q.) | GPT-4 / GPT-3.5 | 77.55% / 50.51% | 56% |
| Infectious diseases PES 2024 (Cureus 2025, 119 q.) | DeepSeek-R1 / GPT-4o | 73.95% / 71.43% | 60.5% |
| Ophthalmology PES (Cureus 2025, 120 q.) | GPT-4o | 78.3% | not stated |
| Vascular surgery PES 2025 (Cureus 2025, 120 q.) | GPT-4o | 73.3% | 60% |
| Thoracic surgery PES (KiTP 2025, 120 q.) | GPT-3.5 | 42.5% | 60% |
| 297 PES exams (arXiv 2405.01589) | gpt-4-0125 / GPT-3.5 | passed 222 of 297 / passed 0 | per exam |

A widely repeated "GPT-4o 77.5%" figure does not appear in any of the primary papers found.

## 4. Tokenization cost

### 4.1 Tokens needed relative to English, FLORES-200, cl100k_base (GPT-4) — V

Source: Petrov et al., NeurIPS 2023; raw CSV in the [tokenization-fairness repository](https://github.com/AleksandarPetrov/tokenization-fairness).

| PL | HR | SL | CS | SK | RU | BG | SR | UK |
|---|---|---|---|---|---|---|---|---|
| 1.91× | 1.85× | 1.88× | 2.11× | 2.14× | 2.49× | 2.64× | 2.92× | 3.00× |

Polish relative to English with other tokenizers:

| GPT-2 | Qwen | LLaMA | mT5 | Glot500 | XLM-R |
|---|---|---|---|---|---|
| 2.69× | 1.76× | 1.70× | 1.31× | 1.23× | 1.19× |

### 4.2 Tokens per word on the preamble of the Polish Constitution (arXiv 2604.10799, Table 1, SELF) — V

| Tokenizer | PL | EN |
|---|---|---|
| Bielik 11B v3 (original) | 3.22 | 1.28 |
| Llama 3.2 | 2.81 | 1.17 |
| Qwen3 | 2.69 | 1.17 |
| Gemma 3 | 2.20 | 1.20 |
| EuroLLM | 1.88 | 1.27 |
| APT4 (Polish) | 1.62 | 1.98 |

**What it means in practice.** For the same content, a Polish prompt uses roughly 1.7–1.9× more context window and API cost than an English one with modern multilingual tokenizers.

## 5. Other Slavic languages

| Language | Source | Result |
|---|---|---|
| Czech, Russian, Ukrainian, Serbian | MMLU-ProX (arXiv 2503.10497, Table 3, V) | Gap vs English: Qwen3-235B-Think −0.2 (CS) to −3.7 (RU); GPT-4.1 −2.3 (CS) to −8.6 (RU); o4-mini −0.2 (CS) to −11.7 (RU). Polish is not included. |
| Bulgarian | BgGPT 1.0 (arXiv 2412.10893, Table 1, V) | EN/BG averages: Llama-3.1-70B 77.75/61.49; Qwen2.5-72B 77.53/58.25; BgGPT-27B 75.12/64.67 |
| Slovak | sk-bench (arXiv 2610.09152, Table 1, P) | Overall: Gemini 3.1 Pro 72.6; GPT-5.5 71.0; Claude Opus 4.6 68.1; gpt-oss-120B 60.0; Llama 3.3 70B 53.1 |
| Ukrainian | ZNO-Vision (arXiv 2411.14647, P) | Exam test accuracy: Gemini Pro 0.675; Claude 3.5 Sonnet 0.643; GPT-4o 0.470 |
| Russian | MERA leaderboard v1.2.0 (P) | GPT-4o overall 0.642 (place 45 at time of reading) |
| BCMS, Slovenian, Macedonian | arXiv 2511.07989 (P) | GPT-5 macro-F1 gap vs English: 0.02 (sentiment), 0.05 (topic) |
| Czech | Aspect-based sentiment (arXiv 2508.07860, P) | Czech prompts match English on classification and are weaker on extraction. GPT-4o mini, TASD zero-shot: 35.5 (EN prompt) vs 24.2 (CS prompt) |

**Data gaps.**

- No verified GPT-4-class number exists for Czech BenCzechMark. That benchmark covers open-weights models only, and its metric is a win rate, not accuracy.
- No verified Slovak MMLU result was found.
- Per-model ZNO-Eval scores could not be read.

## 6. Prompt language

- No primary study was found that directly compares prompting in Polish and in English with modern LLMs.
- Translating the input to English helped GPT-3.5 on NLP tasks (arXiv 2403.10258): on MGSM, high-resource languages went from 44.4 in the native language to 74.7 after translation. The authors note that translation "is not universally optimal".
- For code generation, English prompts do not consistently give the best functional correctness (arXiv 2607.14816; Polish was not tested).
- Mechanistic work (Wendler et al. 2024; Schut et al. 2025) suggests that models make key decisions in a representation closest to English.

## 7. Gaps and caveats

- No independent study compares PLLuM with GPT-4o on general Polish benchmarks.
- The SpeakLeash Open PL leaderboard data was not accessible (HTTP 401).
- Global-MMLU includes Polish only as a machine translation, and its per-language accuracy could not be retrieved.
- Self-reported results (SELF) come from the model developers and may favour their own models.
- Many studies use 2023–2024 models. The gap for frontier models is shrinking, so older numbers overstate it for current models.

## Corrections made during verification

- **Rosoł et al.:** the "GPT-4 80.7% / GPT-3.5 56.6%" figures circulating in search snippets are not in the published paper.
- **Codesota:** its "Bielik 9.43" on Polish MT-Bench is the Extraction column, not the overall score of 8.56.
- **OneRuler:** "Ukrainian 89%" is a single-model value (o3-mini-high at 128K), not an average across models.
- **MERA:** GPT-4o is at place 45, not 26.
- **ACL 2026.unlp-1.12:** this paper is not ZNO-Eval.
- **BenCzechMark:** the Bielik 49.5 is a Duel Win Score, not accuracy.
- **Occiglot tokenizer post:** it contains no GPT-4 row.
- **MMLU-ProX anomaly:** GPT-4o's Russian score (62.0) is above its English score (59.9). This is confirmed in two tables of the source.

## Sources (primary)

| ID | Source | Link |
|---|---|---|
| 2412.00559 | Polish–English medical benchmark | https://arxiv.org/abs/2412.00559 |
| Rosoł 2023 | GPT-4 on LEK | https://doi.org/10.1038/s41598-023-46995-z |
| 2308.16884 | Belebele | https://arxiv.org/abs/2308.16884 |
| 2410.08928 | EU20 benchmarks | https://arxiv.org/abs/2410.08928 |
| 2503.01996 | OneRuler | https://arxiv.org/abs/2503.01996 |
| 2503.00995 | PLCC | https://arxiv.org/abs/2503.00995 |
| 2501.02266 | LLMzSzŁ | https://arxiv.org/abs/2501.02266 |
| LLMzSzŁ leaderboard | Leaderboard | https://huggingface.co/spaces/amu-cai/LLMZSZL_Leaderboard |
| 2505.02410 | Bielik 11B v2 technical report | https://arxiv.org/abs/2505.02410 |
| 2604.10799 | Bielik v3 / APT4 tokenizer | https://arxiv.org/abs/2604.10799 |
| 2511.03823 | PLLuM | https://arxiv.org/abs/2511.03823 |
| 2603.10640 | Polish reasoning puzzles | https://arxiv.org/abs/2603.10640 |
| Cureus 2024 | LEK study (Jaworski et al.) | https://pmc.ncbi.nlm.nih.gov/articles/PMC11366403/ |
| Cureus 2025 | Infectious diseases PES | https://pmc.ncbi.nlm.nih.gov/articles/PMC12102586/ |
| Cureus 2025 | Ophthalmology PES | https://pmc.ncbi.nlm.nih.gov/articles/PMC12392048/ |
| Cureus 2025 | Vascular surgery PES | https://pmc.ncbi.nlm.nih.gov/articles/PMC12548761/ |
| 2405.01589 | 297 PES exams | https://arxiv.org/abs/2405.01589 |
| Petrov 2023 | Tokenization fairness | https://arxiv.org/abs/2305.15425 |
| Petrov 2023 | Raw data | https://github.com/AleksandarPetrov/tokenization-fairness |
| 2503.10497 | MMLU-ProX | https://arxiv.org/abs/2503.10497 |
| 2412.10893 | BgGPT | https://arxiv.org/abs/2412.10893 |
| 2610.09152 | sk-bench | https://arxiv.org/abs/2610.09152 |
| 2411.14647 | ZNO-Vision | https://arxiv.org/abs/2411.14647 |
| MERA | Submission 11642 | https://mera.a-ai.ru/en/submits/11642 |
| 2511.07989 | South Slavic classification | https://arxiv.org/abs/2511.07989 |
| 2508.07860 | Czech ABSA | https://arxiv.org/abs/2508.07860 |
| 2403.10258 | Is Translation All You Need? | https://arxiv.org/abs/2403.10258 |
| 2607.14816 | Multilingual code prompts | https://arxiv.org/abs/2607.14816 |
| OpenAI MMMLU | Dataset card | https://huggingface.co/datasets/openai/MMMLU |
| Anthropic | Multilingual support | https://platform.claude.com/docs/en/build-with-claude/multilingual-support |
