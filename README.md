# eval-regression-viewer

Drop two eval-run files (JSONL, JSON array, or CSV) and get a case-by-case regression diff: new failures, new passes, score deltas, and flaky cases, all in your browser.

**Live demo:** https://0xelitesystem.github.io/eval-regression-viewer/

## Features

- **Three input formats.** JSONL (one object per line, blank lines tolerated), JSON arrays (including common wrappers like `results` or `cases`), and CSV with a from-scratch parser that handles quoted fields and commas inside quotes.
- **Drag-drop, file picker, or paste.** Every run slot accepts a dropped file, a picked file, or pasted text. Dropping multiple files at once fills the open slots in order.
- **2 to 6 runs.** Run A is the baseline, Run B is the candidate, and optional extra runs feed flake detection.
- **Auto field mapping with override.** The case-id field (`id`, `case_id`, `test_id`, `name`, `question`, `input`) and outcome field (`pass`, `passed`, `correct`, `score`, `grade`, `result`) are auto-detected, shown in dropdowns built from your actual keys, and overridable. Booleans and pass/fail strings read as pass/fail; numbers read as scores against a configurable pass threshold.
- **A real diff engine.** Cases are joined on id and categorized: new failure, new pass, regressed (score dropped beyond a configurable delta), improved, unchanged, only-in-baseline, only-in-candidate. With 3+ runs, a case whose outcome flips back and forth is flagged flaky.
- **Summary tiles.** New failures, new passes, net score delta (or pass-rate delta when outcomes are boolean), flaky count, and coverage change.
- **A workable results table.** Filter chips per category, sorting by case id or delta, and expandable rows that show the raw records from every run side by side.
- **Exports.** Copy a markdown report (summary table plus per-category case lists) or download the full diff as CSV.
- **Demo runs built in.** One click loads three embedded runs with real regressions and one flaky case, so you can see the whole flow before touching your own data.
- **Dark theme by default**, light theme a click away, preference persisted locally. No external dependencies.

## How it works

1. Load a baseline run and a candidate run (plus optional reruns). Parsing happens in your browser with the FileReader API; format is detected from the file extension, then by sniffing the content.
2. Check the detected field mapping. Records are joined across runs on the case-id field, and the outcome field decides pass/fail: booleans and strings like `pass`/`fail` directly, numeric scores via the pass threshold.
3. The diff engine categorizes every case. A score move beyond the regression-delta threshold counts as regressed or improved; a pass flip counts as a new failure or new pass; cases missing on one side are tracked as coverage changes. With three or more runs, an outcome that flips at least twice across runs is flagged flaky.
4. Read the tiles, drill into a category with the filter chips, expand rows to compare raw records, then copy the markdown report into a PR or download the CSV.

This tool is the missing "read the results" step for the eval repos in the same portfolio: run your suites with [claude-eval-harness](https://github.com/0xelitesystem/claude-eval-harness), build cases from [llm-eval-datasets-starter](https://github.com/0xelitesystem/llm-eval-datasets-starter), grade with [rag-evaluation-rubrics](https://github.com/0xelitesystem/rag-evaluation-rubrics), then diff the runs here.

## Use

1. Load a baseline run (Run A) and a candidate run (Run B) by dropping files, using Choose file, or pasting text. Or click Load demo runs.
2. Optionally add more runs with + Add run (flake detection).
3. Check the case id and outcome fields in Field mapping, and set the pass threshold and regression delta if your outcomes are scores.
4. Read the summary tiles and filter the results table by category.
5. Click Copy markdown report or Download diff CSV.

## Why this exists

Comparing two eval runs by eye hides which cases regressed and which are just flaky. This joins the runs case by case in a single HTML file with no upload and no tracking, under the MIT license, so eval data never leaves your machine.

## Privacy

Everything runs in your browser. Files are read locally with the FileReader API and never uploaded; there are no analytics, no network requests, and no external dependencies. The only thing stored is your theme preference, in localStorage.

## Run locally

```
git clone https://github.com/0xelitesystem/eval-regression-viewer
cd eval-regression-viewer
```

Open `index.html` in a browser. Or serve the folder with `python -m http.server` and visit http://localhost:8000.

## Build

No build step. The whole tool is one `index.html` file with inline CSS and JavaScript, and no dependencies.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT
