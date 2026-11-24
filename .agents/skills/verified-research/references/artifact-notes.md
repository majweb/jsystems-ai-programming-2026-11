# Interactive artifact notes

A working reference implementation from the first run of this skill lives at
`docs/research/llm-polish-slavic/report-pl.html` in this repository — reuse its structure (data block, i18n dictionary, section renderers) rather than starting from scratch.

## Structure

1. Header: eyebrow (report type · date · "primary sources only"), title, 2-sentence lede, 4 stat tiles with the headline numbers.
2. Numbered evidence sections (the numbers are a real sequence: they match the report sections). Each = heading, one-paragraph claim, a `.figure` with controls, chart, note, and a monospace source line with status label.
3. Myths / facts / gaps cards.
4. Method: steps (agents, iterations, languages, verifiers) + sources table with V/P badges.
5. Footer: status label legend and path to the Markdown report.

## One template, two languages

- Keep all copy in `const T = {pl:{…}, en:{…}}[LANG]`, data labels as `{pl:…, en:…}` objects resolved by a `tx()` helper, numbers formatted with `Intl.NumberFormat(LANG)`.
- Generate the second file: `sed -e 's|<title>A</title>|<title>B</title>|' -e 's|^const LANG = "pl";|const LANG = "en";|' report-pl.html > report-en.html`.
- Syntax-check both: extract the inline `<script>` and run it through `new Function(js)` in node.

## Pitfalls seen in practice

- **Clipped axis labels**: Chart.js measures labels before web fonts load. Re-render on `document.fonts.ready.then(rerender)`.
- **`hidden` ignored**: a class with `display:flex` overrides the `hidden` attribute in a local preview. Add `[hidden]{display:none!important}`.
- **Theme**: read colours from CSS tokens with `getComputedStyle` at draw time, and re-render on `prefers-color-scheme` change and on `data-theme` mutation (MutationObserver), so charts follow light/dark.
- **Chart library**: `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js` (pinned). Keep the page usable without it (tables and HTML bars still render).
- **Anomalous rows**: leave them out of charts and explain why in the note (e.g. a model scoring higher in the target language than the baseline only in one source).
- **Mixed metrics**: never plot win rates, ratios and accuracies on one axis; give each its own tab.
