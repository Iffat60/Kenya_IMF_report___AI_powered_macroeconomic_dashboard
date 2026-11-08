# Kenya_IMF_report___AI_powered_macroeconomic_dashboard
# Kenya IMF Article IV: LLM-Based PDF-to-Dashboard Pipeline

This repository contains the code for an LLM-assisted pipeline that extracts, summarises and evaluates macroeconomic and financial stability information from the IMF's *Kenya: 2023 Article IV Consultation* (IMF Country Report No. 24/13, January 2024).

The pipeline reads the report PDF, cleans and chunks its text, builds a FAISS vector index with sentence embeddings, extracts six numerical indicators, summarises the report's sections, answers questions with retrieval-augmented generation (RAG), evaluates the output, and produces a four-panel dashboard.

## Contents

| File | Purpose |
|---|---|
| `Kenya_IMF_LLM_Assignment.ipynb` | The full pipeline, to run from top to bottom |
| `requirements.txt` | Python packages used by the notebook |
| `LICENSE` | MIT License |

The report PDF is not included in this repository. It is a public IMF document and can be downloaded from the IMF eLIBRARY (IMF Country Report No. 24/13).

## Models used

| Stage | Model |
|---|---|
| Indicator extraction | Gemini 3.5 Flash Lite (`gemini-3.5-flash-lite`) |
| Section and executive summaries | Gemini 3.5 Flash Lite (`gemini-3.5-flash-lite`) |
| RAG answers (RAGAS evaluation) | Gemini 3.5 Flash (run 1) and Gemini 3.5 Flash Lite (run 2) |
| LLM-as-a-judge | Gemma 4 26B A4B (`gemma-4-26b-a4b-it`) |
| Embeddings | `all-MiniLM-L6-v2` (Sentence Transformers) |

The model names in the notebook can change if Google updates its models. The outputs record the model used, so check the printed model line when you re-run it.

## Requirements

- A Google account with access to Google Colab.
- A Gemini API key, stored in Colab Secrets as `GEMINI_API_KEY`.
- The report PDF, uploaded to a folder called `data/` in the Colab session, with the file name `1kenea2024001.pdf`. Any single PDF in `data/` will be used if the name differs.

## How to run

1. Open the notebook in Google Colab.
2. In the Secrets panel (key icon), add a secret named `GEMINI_API_KEY` containing your key, and turn on notebook access.
3. Create a `data/` folder in the Files panel and upload the report PDF into it.
4. Run the install cell, then restart the runtime if Colab asks.
5. Run all cells from top to bottom: **Runtime > Run all**.

Cached model outputs are not included, so a full run makes API calls. Each stage saves its results, so if a run stops (for example on a rate limit), you can run the cell again and it will resume from the last saved step.

## Outputs

| File | Contents |
|---|---|
| `indicators.json` | Extracted indicators, the page each came from, and any fallback reasons |
| `chapter_summaries.json` | Section summaries |
| `thematic_counts.json` | Theme counts by section |
| `qualitative.json` | Growth drivers and vulnerabilities, with pages |
| `kenya_economic_dashboard.png` | Four-panel dashboard, 300 dpi |

## Limitations

- The indicator extraction is a grounding test on the evidence supplied in the prompt. It does not search the whole report.
- The NPL ratio is stated as 15.0% in the report's narrative and 12.7% in its table. The notebook reports the narrative value and records the table value beside it.
- The two RAGAS scores come from two different answers, generated under two model settings. They are not a model comparison.
- Keyword theme counts show how much retrieved evidence each theme received, not how much attention the report gives it.

## Licence

This project is released under the MIT Licence. See `LICENSE`.

The IMF report used in this project is a public document published by the International Monetary Fund. It is not covered by this licence and is not included in this repository.
