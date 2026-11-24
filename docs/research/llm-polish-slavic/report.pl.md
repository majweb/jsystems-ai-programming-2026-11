# Jakość LLM po polsku i w innych językach słowiańskich

Raport oparty na danych źródłowych, stan na 24.11.2026. Wersja angielska: [report.en.md](report.en.md).

## Zakres i metoda

**Pytanie badawcze:** jak dobrze duże modele językowe (LLM) radzą sobie po polsku i w innych językach słowiańskich w porównaniu z angielskim?

**Zasady doboru źródeł.** Cytujemy wyłącznie źródła pierwotne: artykuły z benchmarkami, raporty techniczne, oficjalne leaderboardy, karty zbiorów danych i recenzowane badania. Prasa, blogi i media społecznościowe posłużyły tylko do znalezienia tych źródeł. Każdą liczbę w raporcie odczytano bezpośrednio ze źródła pierwotnego.

**Proces.**

- **Badanie:** 6 agentów badawczych (Claude Haiku) o rozłącznych zakresach:
  - A1: polskie benchmarki;
  - A2: polskie modele i ich raporty techniczne;
  - A3: benchmarki wielojęzyczne;
  - A4: inne języki słowiańskie;
  - A5: dokumentacja producentów modeli i tokenizacja;
  - A6: źródła publikowane po polsku, egzaminy oraz wpływ języka promptu.
- **Iteracje:** A1, A2, A3, A5 i A6 przeszły po 2 iteracje, A4 przeszedł 3. Każda kolejna iteracja uwzględniała feedback koordynatora.
- **Wyszukiwanie w językach narodowych:** A4 szukał po czesku, słowacku, ukraińsku, rosyjsku, słoweńsku, chorwacku, serbsku, bośniacku i bułgarsku. A6 szukał po polsku.
- **Weryfikacja:** 3 niezależnych weryfikatorów (V1–V3) sprawdziło komórka po komórce 14 kluczowych tabel. Nie znaleźli rozbieżności w wartościach. Poprawki opisano w sekcji „Korekty wprowadzone podczas weryfikacji”.
- **Surowe raporty agentów:** katalog [agent-reports/](agent-reports/).

**Oznaczenia statusu w tabelach:**

- **V** – wynik potwierdzony przez drugiego, niezależnego agenta;
- **P** – wynik odczytany ze źródła pierwotnego przez jednego agenta;
- **SELF** – wynik podany przez twórców danego modelu.

**Ograniczenie metody:** wszystkie wartości odczytano przez narzędzie WebFetch, które streszcza pobrane strony. Plików PDF nie dało się przetworzyć lokalnie. Liczby, które weryfikatorzy odczytali ponownie, zgadzają się w co najmniej dwóch odczytach.

## Najważniejsze wnioski

1. **Najmocniejsze modele są po polsku bliskie parytetu z angielskim w zadaniach wiedzowych. Mniejsze i starsze modele nie.**
   - Egzamin LEK: GPT-4o ma 89,96% po polsku i 88,69% po angielsku.
   - Qwen2.5-7B na tym samym egzaminie ma 51,92% po polsku i 67,73% po angielsku, czyli o 15,8 pkt proc. mniej.
   - W Belebele (czytanie ze zrozumieniem) każdy model z 2023 r. wypada po polsku o 7,5–17,3 pkt gorzej niż po angielsku.
2. **Różnica maleje wraz z postępem modeli.** Na egzaminie LEK:
   - GPT-3.5 ma 54,8% po polsku i 60,3% po angielsku;
   - GPT-4 ma 79,7% w obu językach (Sci Rep 2023).
3. **W znajomości polskiego języka i kultury prowadzą modele komercyjne.** Wyniki benchmarku PLCC:

   | Model | Wynik |
   |---|---|
   | Gemini-Exp-1206 | 83,0% |
   | Claude 3.5 Sonnet | 82,7% |
   | GPT-4o | 81,3% |
   | Bielik 2.3 (11B), najlepszy model polski | 62,2% |
   | Llama-3.1-405B | 60,0% |

4. **Polskie modele otwarte konkurują w swojej klasie wielkości, nie z modelami czołowymi.**
   - Na leaderboardzie LLMzSzŁ (polskie egzaminy państwowe) Qwen2.5-72B ma 69,1, Llama-PLLuM-70B 64,4, a Bielik-11B-v2.6 59,2.
   - Na tym leaderboardzie nie ma żadnego modelu zamkniętego.
5. **Polski tekst kosztuje mniej więcej dwa razy więcej tokenów niż angielski.**
   - Tokenizer GPT-4 (cl100k) potrzebuje dla polskiego 1,91 razy więcej tokenów niż dla angielskiego, mierząc na tych samych zdaniach z korpusu FLORES-200.
   - Dla czeskiego jest to 2,11 razy, dla rosyjskiego 2,49 razy, a dla ukraińskiego 3,00 razy więcej.
   - Polski tokenizer APT4 zmniejsza liczbę tokenów na polskie słowo z 3,22 do 1,62 (SELF, pomiar na jednym tekście).
6. **Hasło „polski to najlepszy język dla AI” przesadnie uogólnia jeden wynik.**
   - W OneRuler, w zadaniu wyszukiwania informacji w długim tekście (NIAH), polski jest pierwszy na 26 języków z wynikiem 88%. Angielski jest szósty z wynikiem 83,9%.
   - To jednak tylko syntetyczne zadania wyszukiwania, a nie ogólna jakość promptowania.
7. **Producenci modeli rzadko podają wyniki dla polskiego.**
   - Polskiego wiersza nie ma w tabelach OpenAI MMMLU (14 języków), Anthropic Global-MMLU (14 języków), Llama 3.1 (8 języków) ani w tabeli tokenizacji GPT-4o (20 języków).
   - Polski nie występuje też w MMLU-ProX (29 języków) ani w Global-MMLU-Lite.
8. **W innych językach słowiańskich widać ten sam wzorzec.**
   - W BgGPT modele otwarte tracą po bułgarsku 10–22 pkt względem angielskiego.
   - W sk-bench (55 modeli) autorzy piszą, że po słowacku żaden model otwarty nie dorównuje modelom dostępnym przez komercyjne API.
   - W MMLU-ProX czołowe modele rozumujące tracą po czesku, serbsku i ukraińsku tylko 0,2–3,7 pkt.

## 1. Polski i angielski na tych samych pytaniach

### 1.1 Egzaminy lekarskie w równoległych wersjach PL i EN (arXiv 2412.00559v2, Tabela 4) – V

| Model | LEK PL | LEK EN | Różnica (pkt) | LDEK PL | LDEK EN |
|---|---|---|---|---|---|
| gpt-4o-2024-08-06 | 89,96 | 88,69 | +1,27 | 73,89 | 72,51 |
| Llama-3.1-70B-Instruct | 80,94 | 82,60 | −1,66 | 61,75 | 63,17 |
| Qwen2.5-72B-Instruct (P) | 76,87 | 81,93 | −5,06 | 58,35 | 63,33 |
| Mistral-Large-Instruct-2407 | 76,75 | 82,40 | −5,65 | 56,29 | 62,14 |
| gpt-4o-mini-2024-07-18 | 75,88 | 75,92 | −0,04 | 54,94 | 56,88 |
| Bielik-11B-v2.2-Instruct | 62,36 | 56,98 | +5,38 | 43,20 | 42,88 |
| Qwen2.5-7B-Instruct | 51,92 | 67,73 | −15,81 | 43,71 | 48,38 |
| Llama-3.2-3B-Instruct (P) | 39,22 | 52,08 | −12,86 | 32,16 | 36,87 |

Autorzy podsumowują, że modele „radzą sobie istotnie lepiej z pytaniami po angielsku”. Wyjątkami są GPT-4o i Bielik. Artykuł nie podaje, ile pytań obejmuje podzbiór z Tabeli 4.

### 1.2 LEK 2022–2023: GPT-3.5 i GPT-4 (Rosoł i in., Sci Rep 13:20512, 2023) – V

| Model | Polski | Angielski | Próg zdania | Studenci |
|---|---|---|---|---|
| GPT-4 | 79,7% | 79,7% | 56% | 83,0–84,8% |
| GPT-3.5 | 54,8% | 60,3% | 56% | — |

Często powtarzanej wartości „80,7%” nie ma w opublikowanym artykule.

### 1.3 Belebele, czytanie ze zrozumieniem (arXiv 2308.16884, ACL 2024, Tabela 7) – V dla pierwszych trzech wierszy

| Model | EN | PL | Różnica | CS | RU | UK | SK | BG | HR | SL | SR |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Llama 2 70B, 5-shot | 90,9 | 79,2 | −11,7 | 81,1 | 82,7 | 80,8 | 75,2 | 80,4 | 79,8 | 76,7 | 81,0 |
| GPT-3.5-turbo, 0-shot | 87,7 | 74,7 | −13,0 | 76,9 | 78,4 | 72,8 | 77,3 | 76,0 | 78,4 | 77,4 | 74,8 |
| Llama-2-chat, 0-shot | 78,8 | 61,7 | −17,1 | 65,0 | 67,0 | 65,7 | 58,8 | 65,0 | 64,7 | 62,4 | 65,1 |
| Llama 1, 5-shot (P) | 82,5 | 67,0 | −15,5 | 70,7 | 73,1 | 69,7 | 60,4 | 69,3 | 66,9 | 65,6 | 70,7 |
| Falcon, 5-shot (P) | 77,2 | 59,9 | −17,3 | 65,0 | 48,1 | 41,9 | 57,0 | 41,9 | 48,7 | 43,7 | 40,2 |
| InfoXLM-L, translate-train (P) | 81,2 | 73,7 | −7,5 | 76,2 | 76,8 | 74,9 | 76,7 | 75,3 | 75,3 | 75,4 | 76,1 |

### 1.4 EU20-MMLU, tłumaczenie maszynowe DeepL (arXiv 2410.08928v2, Tabela 1, Llama-3.1-70B-Instruct) – V

| EN | PL | CS | SL | SK | BG |
|---|---|---|---|---|---|
| 0,82 | 0,78 | 0,78 | 0,74 | 0,68 | 0,67 |

Autorzy zastrzegają, że na wyniki prawdopodobnie wpływa jakość tłumaczenia.

### 1.5 OneRuler, długi kontekst (arXiv 2503.01996v2, COLM 2025) – V

**Wyniki:**
- Średnia ze wszystkich modeli przy kontekście 64K–128K tokenów: polski jest 1. na 26 języków (88% w NIAH), angielski 6. (83,9%).
- Jeden model, o3-mini-high, przy kontekście 128K tokenów: angielski 67%, polski 92%, ukraiński 89%.
- Wyniki dla pozostałych języków są tylko na wykresach w artykule, bez liczb w tekście.

**Zastrzeżenia:**
- Są to syntetyczne zadania wyszukiwania informacji.
- Dane polskie i japońskie tłumaczyła ta sama osoba.
- Polskim tekstem wypełniającym jest powieść z 1934 r.

## 2. Polskie benchmarki i leaderboardy

### 2.1 PLCC: kompetencje językowe i kulturowe (arXiv 2503.00995, OPI-PIB) – V

Benchmark ma 600 pytań w 6 kategoriach. Wynik to procent poprawnych odpowiedzi.

| Model | Wynik | Typ |
|---|---|---|
| Gemini-Exp-1206 | 83,00 | komercyjny |
| Claude-3.5-Sonnet | 82,67 | komercyjny |
| GPT-4o | 81,33 | komercyjny |
| Gemini-2.0-Flash (P) | 74,17 | komercyjny |
| Claude-3-Opus (P) | 73,83 | komercyjny |
| DeepSeek-V3 | 69,17 | otwarty |
| GPT-4-turbo | 67,00 | komercyjny |
| Bielik-2.3 (11B) | 62,17 | polski |
| Llama-3.1-405B | 60,00 | otwarty |
| Mistral-Large-2411 (P) | 52,00 | otwarty |
| Llama-3.3-70B (P) | 48,83 | otwarty |
| Bielik-0.1 (7B) (P) | 46,67 | polski |
| GPT-3.5-turbo | 43,33 | komercyjny |
| Qwen-2.5-72B (P) | 39,17 | otwarty |
| Llama-3.1-8B | 22,67 | otwarty |

Najtrudniejszą kategorią jest gramatyka. Nawet GPT-4o ma w niej 67 pkt, a w pozostałych kategoriach 80–89 pkt.

### 2.2 LLMzSzŁ: polskie egzaminy państwowe, ok. 19 tys. pytań (plik JSON leaderboardu i arXiv 2501.02266) – P

Leaderboard obejmuje tylko modele otwarte.

| Model | Trafność |
|---|---|
| Qwen2.5-72B-Instruct | 69,06 |
| Mistral-Large-Instruct-2407 | 67,17 |
| Llama-3.3-70B-Instruct | 67,13 |
| Llama-3.1-70B-Instruct | 66,59 |
| Llama-PLLuM-70B-chat | 64,42 |
| PLLuM-8x7B-nc-chat | 60,52 |
| Bielik-11B-v2.6-Instruct | 59,23 |
| Bielik-11B-v2.1-Instruct | 57,52 |
| trurl-2-13b | 40,22 |
| Qra-13b | 34,85 |

### 2.3 Pozostałe polskie benchmarki

| Benchmark | Wyniki | Źródło |
|---|---|---|
| Open PL LLM Leaderboard (5-shot, modele instruct) | Qwen2.5-72B 67,92; Llama-3.3-70B 66,40; Bielik-11B-v2.3 65,71; Qwen2.5-14B 59,91 | arXiv 2505.02410v2, Tab. 8 (SELF, V) |
| Polish EQ-Bench | gpt-4o 75,15; gpt-4-turbo 74,59; Bielik-11B-v3 71,20; gpt-4o-mini 71,15 | arXiv 2604.10799, Tab. 3 (SELF, P) |
| CPTUB, rozumienie złożonego tekstu (skala ok. 0–5) | gemini-2.0-flash 4,29; Bielik-PL-11B-v3 3,80; Bielik-11B-v3 3,73 | arXiv 2604.10799, Tab. 4 (SELF, P) |
| Polish MT-Bench (skala 0–10, ocenia GPT-4) | Qwen2.5-32B 8,86; Mistral-Small-2501 8,72; Bielik-11B-v2.3 8,56 | arXiv 2505.02410v2, Tab. 10 (SELF, P) |
| Polskie zagadki logiczne (111 zadań) | Gemini-3-pro 87; o3 87; gemini-2.5-pro 84; qwen3:8b 68; bielik-r 56 | arXiv 2603.10640, Tab. 2 (P) |
| RAG-IFEval, zadania administracji publicznej | Grok-3 91,1; GPT-4.1 90,2; Llama-PLLuM-70B 89,7; GPT-4o 80,3 | arXiv 2511.03823, Tab. 33 (SELF, benchmark autorów PLLuM) |

## 3. Egzaminy lekarskie LEK i specjalizacyjne PES – P

| Badanie | Model | Wynik | Próg zdania |
|---|---|---|---|
| LEK wiosna 2021 (Cureus 2024, 196 pytań) | GPT-4 / GPT-3.5 | 77,55% / 50,51% | 56% |
| PES choroby zakaźne 2024 (Cureus 2025, 119 pytań) | DeepSeek-R1 / GPT-4o | 73,95% / 71,43% | 60,5% |
| PES okulistyka (Cureus 2025, 120 pytań) | GPT-4o | 78,3% | nie podano |
| PES chirurgia naczyniowa 2025 (Cureus 2025, 120 pytań) | GPT-4o | 73,3% | 60% |
| PES torakochirurgia (KiTP 2025, 120 pytań) | GPT-3.5 | 42,5% | 60% |
| 297 egzaminów PES (arXiv 2405.01589) | gpt-4-0125 / GPT-3.5 | zdane 222 z 297 / zdane 0 | osobny dla każdego egzaminu |

Często cytowanej wartości „GPT-4o 77,5%” nie ma w żadnym ze znalezionych artykułów źródłowych.

## 4. Koszt tokenizacji

### 4.1 Liczba tokenów względem angielskiego, FLORES-200, tokenizer cl100k_base (GPT-4) – V

Źródło: Petrov i in., NeurIPS 2023, surowy plik CSV z repozytorium tokenization-fairness.

| PL | HR | SL | CS | SK | RU | BG | SR | UK |
|---|---|---|---|---|---|---|---|---|
| 1,91× | 1,85× | 1,88× | 2,11× | 2,14× | 2,49× | 2,64× | 2,92× | 3,00× |

Ten sam stosunek (polski do angielskiego) w innych tokenizerach:

| Tokenizer | GPT-2 | Qwen | LLaMA | mT5 | Glot500 | XLM-R |
|---|---|---|---|---|---|---|
| PL/EN | 2,69× | 1,76× | 1,70× | 1,31× | 1,23× | 1,19× |

### 4.2 Tokeny na słowo w preambule Konstytucji RP (arXiv 2604.10799, Tab. 1, SELF) – V

| Tokenizer | PL | EN |
|---|---|---|
| Bielik 11B v3 (oryginalny) | 3,22 | 1,28 |
| Llama 3.2 | 2,81 | 1,17 |
| Qwen3 | 2,69 | 1,17 |
| Gemma 3 | 2,20 | 1,20 |
| EuroLLM | 1,88 | 1,27 |
| APT4 (polski) | 1,62 | 1,98 |

**W praktyce:** przy współczesnych tokenizerach wielojęzycznych prompt o tej samej treści po polsku zajmuje około 1,7–1,9 razy więcej okna kontekstu i kosztuje proporcjonalnie więcej w API niż po angielsku.

## 5. Inne języki słowiańskie

| Język | Źródło | Wynik |
|---|---|---|
| czeski, rosyjski, ukraiński, serbski | MMLU-ProX (arXiv 2503.10497, Tab. 3, V) | Różnica względem angielskiego: Qwen3-235B-Think od −0,2 (CS) do −3,7 (RU); GPT-4.1 od −2,3 (CS) do −8,6 (RU); o4-mini od −0,2 (CS) do −11,7 (RU). Benchmark nie obejmuje polskiego. |
| bułgarski | BgGPT 1.0 (arXiv 2412.10893, Tab. 1, V) | Średnie EN/BG: Llama-3.1-70B 77,75/61,49; Qwen2.5-72B 77,53/58,25; BgGPT-27B 75,12/64,67 |
| słowacki | sk-bench (arXiv 2610.09152, Tab. 1, P) | Wynik ogólny: Gemini 3.1 Pro 72,6; GPT-5.5 71,0; Claude Opus 4.6 68,1; gpt-oss-120B 60,0; Llama 3.3 70B 53,1 |
| ukraiński | ZNO-Vision (arXiv 2411.14647, P) | Trafność na części testowej: Gemini Pro 0,675; Claude 3.5 Sonnet 0,643; GPT-4o 0,470 |
| rosyjski | leaderboard MERA v1.2.0 (P) | GPT-4o: wynik ogólny 0,642, 45. miejsce w chwili odczytu |
| BCMS, słoweński, macedoński | arXiv 2511.07989 (P) | GPT-5 względem angielskiego: różnica macro-F1 0,02 (sentyment) i 0,05 (klasyfikacja tematyczna) |
| czeski | analiza sentymentu aspektowego (arXiv 2508.07860, P) | Prompt czeski daje wyniki podobne do angielskiego w klasyfikacji i słabsze w ekstrakcji. GPT-4o mini w zadaniu TASD (0-shot): 35,5 z promptem EN i 24,2 z promptem CS. |

**Braki w danych:**
- Brak zweryfikowanych wyników modeli klasy GPT-4 w czeskim BenCzechMark. Ten benchmark obejmuje tylko modele otwarte, a jego miarą jest odsetek wygranych porównań, a nie trafność.
- Brak słowackiej wersji MMLU.
- Nie udało się odczytać wyników poszczególnych modeli w ZNO-Eval.

## 6. Język promptu

- Nie znaleziono badania źródłowego, które bezpośrednio porównuje promptowanie po polsku i po angielsku na współczesnych modelach.
- Tłumaczenie wejścia na angielski pomagało GPT-3.5 w zadaniach NLP (arXiv 2403.10258). W zadaniach matematycznych MGSM dla języków z dużą ilością danych wynik wzrósł z 44,4 do 74,7. Autorzy zaznaczają jednak, że tłumaczenie „nie jest uniwersalnie optymalne”.
- Przy generowaniu kodu prompty angielskie nie dają za każdym razem najlepszej poprawności (arXiv 2607.14816). Polskiego w tym badaniu nie testowano.
- Badania mechanistyczne (Wendler i in. 2024, Schut i in. 2025) wskazują, że kluczowe decyzje modelu zapadają w reprezentacji najbliższej angielskiemu.

## 7. Luki i zastrzeżenia

- Brak niezależnego porównania PLLuM z GPT-4o na ogólnych polskich benchmarkach.
- Dane leaderboardu Open PL (SpeakLeash) były niedostępne (błąd HTTP 401).
- Global-MMLU zawiera polski tylko w tłumaczeniu maszynowym. Wyników dla poszczególnych języków nie udało się pobrać.
- Wyniki oznaczone SELF pochodzą od twórców modelu i mogą faworyzować ich własne modele.
- Wiele badań dotyczy modeli z lat 2023–2024. Różnica między polskim a angielskim w najlepszych modelach maleje, więc dla obecnych modeli starsze dane ją zawyżają.

## Korekty wprowadzone podczas weryfikacji

- Rosoł i in.: wartości „GPT-4 80,7% / GPT-3.5 56,6%” krążące w wynikach wyszukiwania nie występują w opublikowanym artykule.
- Codesota: wynik „Bielik 9,43” w Polish MT-Bench to kolumna Extraction. Wynik ogólny wynosi 8,56.
- OneRuler: „ukraiński 89%” to wynik jednego modelu (o3-mini-high przy 128K), a nie średnia.
- MERA: GPT-4o zajmuje 45. miejsce, a nie 26.
- Artykuł ACL 2026.unlp-1.12 to nie ZNO-Eval.
- BenCzechMark: wynik Bielika 49,5 to Duel Win Score (odsetek wygranych porównań z innymi modelami), a nie trafność.
- Post Occiglot o tokenizacji nie zawiera GPT-4.
- MMLU-ProX: GPT-4o ma po rosyjsku 62,0, a po angielsku 59,9. Ta anomalia występuje w dwóch tabelach źródła.

## Źródła

Pełna lista linków znajduje się w [report.en.md](report.en.md#sources-primary).
