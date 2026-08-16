# Great Learning — PGP in AI/ML — Course Materials

Downloaded course materials (lecture notes, notebooks, datasets) for the Great
Learning "Post Graduate Program in Artificial Intelligence & Machine Learning",
organized by course / week / content category, plus a `uv`-managed Python
environment for running the notebooks locally.

See [GreatLearning_Course_Manifest.md](GreatLearning_Course_Manifest.md) for the
original per-course/per-week download inventory this structure was derived from.

## Repo structure

```
<course_slug>/
  week_N/
    lecture_notes/         # slide/PDF lecture material
    lecture_notebooks/      # instructional notebooks
    mls/                    # Mentored Learning Session material
    practice_exercise/      # optional practice case studies
    additional_material/    # supplementary datasets/references
  project/                  # main course project files
  additional_project/       # secondary/bonus project files
_unsorted/                  # files that couldn't be confidently matched to a
                             # manifest row (see below) - review and re-file manually
```

Courses without a real week structure (`01_pre_work/`, `00_program_overview/`) are
kept flat, matching how their content was originally organized.

Course directories are numbered to reflect the actual program curriculum order
(derived from `00_program_overview/AIML Program - Delivery Structure.pdf` and the
official McCombs/UT Austin program brochure), not the order they happened to be
downloaded in:

| # | Directory | Program stage |
|---|---|---|
| 00 | `program_overview` | Orientation / mentor onboarding material |
| 01 | `pre_work` | Preparatory module |
| 02 | `python_foundations` | Module 01 — Python for AI Solutions |
| 03 | `machine_learning` | Module 02 — Predictive Modeling (Linear Regression, Decision Trees, Clustering) |
| 04 | `advanced_machine_learning` | Module 02 — Predictive Modeling (Ensemble Techniques) |
| 05 | `introduction_to_neural_networks` | Module 02 — Predictive Modeling (Neural Networks) |
| 06 | `nlp_with_generative_ai` | Module 03 — Generative AI for NLP |
| 07 | `generative_ai` | Module 04 — Agentic AI for Automation |
| 08 | `model_deployment` | Module 05 — Deploying AI Solutions |
| 09 | `introduction_to_computer_vision` | Self-paced elective (no accessible downloads) |
| 10 | `statistical_learning` | Self-paced elective (no accessible downloads) |
| 11 | `recommendation_systems` | Self-paced elective |
| 12 | `introduction_to_nlp` | Legacy/superseded NLP course |

`09_introduction_to_computer_vision` and `10_statistical_learning` have no local
folder since nothing in those courses was accessible/downloaded at manifest time.

`_unsorted/` holds ~62 files whose manifest row couldn't be confidently matched
to a downloaded file (duplicate/renamed copies, ambiguous titles, or bonus files
from course sections the manifest recorded as locked). Review these manually.

## Environment setup

This project uses [uv](https://docs.astral.sh/uv/) to manage a Python 3.12
virtual environment (`.venv/`), scoped to only the packages actually
imported/pip-installed across the notebooks in this repo.

```bash
# one-time setup (already done if you're reading this after cloning)
uv sync

# activate the venv
source activate.sh          # or: source .venv/bin/activate
```

A Jupyter kernel named **AIML GreatLearning (.venv)** (`aiml-greatlearning`) is
registered for this venv — select it in Jupyter/VS Code when opening any
notebook. If you clone this repo elsewhere, re-register it with:

```bash
uv run python -m ipykernel install --user --name aiml-greatlearning --display-name "AIML GreatLearning (.venv)"
```

Launch Jupyter directly with:

```bash
uv run jupyter lab
```

### Note on dependencies

Some individual notebooks `!pip install` a specific pinned version (each was
authored independently for a standalone Colab run). This repo's
`pyproject.toml` instead declares one dependency set covering everything
actually imported across all notebooks, letting `uv` resolve one mutually
compatible, current version set — so exact versions may differ from what a
given notebook's own install cell pins.

`parler-tts` (used in one bonus TTS-related notebook) was **not** added — it
hard-pins `transformers==4.46.1`, which conflicts with the `transformers`
version needed everywhere else. Install it manually in an isolated environment
if you need that specific notebook.

## Config / secrets

A handful of notebooks (GenAI/agent case studies) read API keys from a local
`config.json` (Colab-style `open("config.json")` in the notebook's own
directory). To keep one source of truth without duplicating secrets:

- **`config.json`** (repo root) — your real keys. Gitignored (`/config.json`
  only, anchored to the root — nested/legit config files elsewhere aren't
  affected). Fill in your actual `OPENAI_API_KEY`, etc. here.
- **`config.json.example`** — placeholder template, committed to git, showing
  which keys are expected.
- Each notebook directory that expects a local `config.json` (`01_pre_work/`,
  `07_generative_ai/mls/`, `06_nlp_with_generative_ai/week_3/mls/`) instead
  contains a **relative symlink** named `config.json` pointing back to the root
  file — so editing the root file updates every notebook's view of it.

## Manifest / provenance

[GreatLearning_Course_Manifest.md](GreatLearning_Course_Manifest.md) records,
for every file, which course/week/category it came from and its download
status. File names in the manifest sometimes differ from the actual downloaded
filenames (Chrome dedup suffixes, differing titles) — files were matched by
content/context rather than exact string match; a few remain in `_unsorted/`
where that couldn't be done with confidence.
