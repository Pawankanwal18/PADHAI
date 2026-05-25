# HELPINGHAND — Exam Question Extractor & Training Dataset

This repository contains tools and data to extract exam questions from PDF files, aggregate them by topic, and produce a CSV dataset suitable for training an exam-question predictor.

Status: Proof-of-concept — extraction script and an example notebook are available.

## Key files
- [scripts/extract_questions_from_pdf.py](scripts/extract_questions_from_pdf.py) — PDF extraction script (uses `pdfminer.six` and `pandas`).
- [notebooks/question_extraction.ipynb](notebooks/question_extraction.ipynb) — runnable notebook that reproduces the pipeline.
 - [data/training-dataset.csv](data/training-dataset.csv) — aggregated questions (output from the script).
 - [data/topics-summary.csv](data/topics-summary.csv) — per-topic counts and statistics.
 - [data/training-template.csv](data/training-template.csv) — CSV header template for guidance.

## Purpose
- Extract questions and headings from PDF exam papers.
- Normalize and deduplicate question text to find repeated questions.
- Produce a labelled CSV format that tracks: sample_question_id, normalized_question, question_text, topic, source_file, occurrence_count, first_line.

## Quickstart

1. Install dependencies (recommended):

```powershell
python -m pip install --user pdfminer.six pandas tqdm
```

2. Run the extraction script on a PDF (example):

```powershell
python scripts/extract_questions_from_pdf.py "CSE-3rd-year.pdf" --out data/training-dataset.csv
```

3. Open the notebook to reproduce and inspect results:

```powershell
jupyter lab notebooks/question_extraction.ipynb
```

## Data format
- `sample_question_id`: short unique id for a normalized question sample.
- `normalized_question`: lowercased, punctuation-removed question used for grouping.
- `question_text`: original extracted question text (one sample per normalized group).
- `topic`: nearest heading or detected section.
- `source_file`: source PDF filename.
- `occurrence_count`: how many times this normalized question appeared.
- `first_line`: approximate line index in the extracted text (for locating the question in the PDF).

This format is friendly for downstream ML training and for simple analytics such as "which questions repeat most".

## Collaboration & committing to Pawan's repo

If you're collaborating and want to push these changes into Pawan's GitHub repository, follow these steps from your local clone. Replace `<PAWAN_REPO_URL>` with the repo URL that Pawan shared.

```bash
# create a feature branch
git checkout -b feature/add-extraction-readme

# stage changes
git add .

# commit with a clear message
git commit -m "Add PDF extraction script, notebook, and dataset; update README"

# add Pawan's remote (only if not already added)
git remote add pawan <PAWAN_REPO_URL>

# push the branch to Pawan's repo
git push pawan feature/add-extraction-readme

# then create a Pull Request on GitHub from that branch
```

Notes:
- Do not push sensitive files (API keys, credentials). This project contains only scripts and sample data.
- If you don't have permission to push directly, fork the repo and open a pull request from your fork.

## Next steps and ideas
- Improve topic detection by training a simple classifier or using heading heuristics.
- Expand normalization (stem/lemmatize) to better group similar questions.
- Create a small web dashboard to query question frequency and export practice tests.

---

If you want, I can now:
- commit these README changes in this workspace and create a local branch, or
- prepare a ready-to-run PR branch and push it to Pawan's repo if you provide the remote URL and credentials.

Contact: add collaborators or mention `@pawan` in the PR description when opening the PR.

## Additional setup: merging PDFs, per-year datasets, and VS Code extensions

1. Install Python dependencies:

```powershell
python -m pip install --user -r requirements.txt
```

2. To merge PDFs by year (combine subjects for each year into one PDF):

```powershell
python scripts/merge_pdfs_by_year.py
# Merged files will be created under the merged-pdfs/ folder
```

3. To extract questions from all PDFs and produce a per-year dataset (preserves per-year counts):

```powershell
python scripts/extract_questions_from_pdf.py . --out "data/training-dataset.csv"
```

4. Recommended VS Code extensions for working with this project:

- `ms-python.python` — Python language support and debugger.
- `ms-toolsai.jupyter` — Jupyter notebook support inside VS Code.
- `ms-python.vscode-pylance` — Fast Python language server (type checking/intellisense).
- `eamodio.gitlens` — Git enhancements (optional but helpful).

Install extensions from the command line (if `code` is on PATH):

```bash
code --install-extension ms-python.python
code --install-extension ms-toolsai.jupyter
code --install-extension ms-python.vscode-pylance
code --install-extension eamodio.gitlens
```

## Notes

- The extraction pipeline now writes a `year` column and groups occurrence counts per-year so questions from different years are not merged together. The default output filename is `data/training-dataset.csv` (human-friendly, no underscores).
- If you want me to also rename notebook and script filenames to remove underscores, tell me and I'll update code references accordingly.
