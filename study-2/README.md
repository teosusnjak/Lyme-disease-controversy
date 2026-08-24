# Study 2: Evidence-Domain Translation

This directory supports:

> Susnjak, T., Brehme, L., Garg, K., Avramovic, G., and Gilbert, L. (2026). Evidence-Domain Translation in the Lyme Disease Controversy over Persistent Post-Treatment Symptoms. *Frontiers in Cellular and Infection Microbiology*, 16. https://doi.org/10.3389/fcimb.2026.1913455

Study 2 builds on the corpus and classification framework developed in Study 1. It examines how model-derived claim orientations are distributed across study-design and evidence-domain tiers, and uses targeted full-text audits to examine how selected clinical and preclinical findings are translated into broader claims.

This repository contains the data, DSPy classifiers, notebooks and results used to collect, classify and analyze scientific abstracts related to the controversy between "Post-Treatment Lyme Disease Syndrome" (PTLDS) and "Chronic Lyme Disease" (CLD).

There are **four** distinct classification stages applied to each abstract, each with its own classifier(s), results and (where available) human ground truth:

1. **Classification (stance)** — is the abstract relevant to the PTLDS/CLD debate, and if so which side does it support? Labels: `Unrelated`, `Animal Study`, `Neutral`, `Supports PTLDS`, `Supports CLD`.
2. **Domain classification (study design)** — what kind of study is this? A `primary_design` (one of `randomized_controlled_trial`, `nonrandomized_controlled_trial`, `prospective_cohort`, `retrospective_cohort`, `case_control`, `cross_sectional`, `case_series`, `case_report`, `systematic_review_or_meta_analysis`, `animal_model`, `in_vitro`, `diagnostic_accuracy_study`, `methods_or_assay_development`, `guideline_or_editorial_or_commentary`, `other_or_unclear`, ...) plus zero or more `secondary_designs` from the same taxonomy.
3. **Editorial classification** — a follow-up step that takes every abstract whose domain classification landed in the `guideline_or_editorial_or_commentary` bucket and decides which of the three it actually is: `editorial`, `commentary`, or `guideline`. No labeled training data existed for this step, so an un-optimized zero-shot prompt classifier is used instead of a GEPA-trained one.
4. **Thematic classification** — for abstracts relevant to the debate, assign one or more of the recurring debate themes (e.g. `Diagnostic Complexity and Uncertainty`, `Therapeutic Controversies and Antibiotic Efficacy`, `Immune Dysregulation and Autoimmune Mechanisms`, `Neurocognitive and Neuropsychiatric Manifestations`, `Active Infection vs. Post-Infectious Immune Activity`, `Patient-Centered Experiences and Advocacy`, `Sociocultural and Ethical Factors`), each theme carrying a vote count (`themes_count`) from the models that assigned it.

Each stage is run independently with three models (Gemini, GPT, Grok), then ensembled (majority vote) into a final label; results are validated against human-annotated ground truth, and `notebooks/final_data.ipynb` merges all four stages into `results/final_classification_results.json`, the end product of the pipeline.

Classifiers are DSPy programs optimized with **GEPA**; notebooks contain the training and inference code; `results/` and `validation/` hold the outputs of each stage.

## Availability and exclusions

Selected raw data not subject to third-party copyright, together with analysis code and compact worked examples, will be made available here.

The source corpus of article abstracts and copyrighted article full texts will not be redistributed. Released records may include persistent identifiers and permitted bibliographic metadata so that readers can retrieve the source material from its original database or publisher. Model outputs must be checked before release and must not contain embedded abstract or full-text content that cannot be redistributed.

See the [repository home page](../) for the article link and abstract.

## Repository structure

```
classifier/     DSPy classifier programs (optimized prompts), one per model
notebooks/      Jupyter notebooks used for training and running the classifiers
results/        Raw output of the classifiers, per model and ensembled
validation/     Human-annotated ground truth for validation and training
```

### `classifier/`

DSPy program dumps (JSON, each with a `predict` and `metadata` section) produced by GEPA optimization — these are the compiled classifiers themselves (prompts + few-shot demos), not classification output, despite some filenames containing "results" or "groundtruth".

- **Stance classification** (root of `classifier/`): one optimized classifier per model, deciding the `Unrelated`/`Animal Study`/`Neutral`/`Supports PTLDS`/`Supports CLD` label.
  - `stuedent_openai_gpt-5-minireflactoropenai_gpt-4o-mini_original_gepa_auto_medium.json` — GPT classifier (student `gpt-5-mini`, reflection model `gpt-4o-mini`).
  - `stuedent_openrouter_google_gemini-2.0-flash-001reflactoropenrouter_google_gemini-2.0-flash-001_original_gepa_auto_medium.json` — Gemini classifier (student/reflection `gemini-2.0-flash-001`).
  - `stuedent_openrouter_x-ai_grok-4-fastreflactoropenrouter_x-ai_grok-4-fast_original_gepa_auto_medium.json` — Grok classifier (student/reflection `grok-4-fast`).
- `domain_classification/` — one optimized classifier per model for the **study-design** classification (`primary_design`/`secondary_designs`), trained against the human ground-truth splits:
  - `classification_results_gemini_groundtruth.json`, `classification_results_gpt_groundtruth.json`, `classification_results_grok_groundtruth.json`.
  - There is **no** separate classifier file for the editorial/commentary/guideline follow-up step: no training data was available for it, so an un-optimized zero-shot prompt classifier is used directly in the corresponding notebooks instead of a GEPA-trained program.
- `thematic_classification/` — classifiers for theme classification:
  - `three_theme_classification_results_gemini_original.json` — Gemini theme classifier.
  - `three_theme_classification_results_grok_original.json` — grok theme classifier (initial version).
  - `three_theme_classification_results_gpt_original_reflacor_gpt-4omini.json` — GPT theme classifier re-optimized with a `gpt-4o-mini` reflection model.


### `notebooks/`

Jupyter notebooks for training and running the classifiers.

- `classification.ipynb` — runs stance classification (`Unrelated`/`Animal Study`/`Neutral`/`Supports PTLDS`/`Supports CLD`) over the full corpus.
- `classificator_training.ipynb` — trains/optimizes the stance classifiers with GEPA.
- `ensembled_classification.ipynb` — combines the per-model stance classification results into a single ensembled label.
- `final_data.ipynb` — merges all four classification stages (stance, study design, editorial, thematic) into the final per-abstract dataset.
- `validation_preprocessing.ipynb` — builds the 300-sample balanced set and other validation/test splits from the labeled data.
- `domain_classification/` — the study-design classification stage (`primary_design`/`secondary_designs`):
  - `domain_classification_{gemini,gpt,grok}.ipynb` — run study-design classification with each model's optimized classifier.
  - `domain_classification_training_{gemini,gpt,grok}.ipynb` — GEPA training/optimization of each model's study-design classifier.
  - `editorial_guidline_commentary_{gemini,gpt,grok}.ipynb` — the editorial follow-up step: reclassify abstracts whose study design was `guideline_or_editorial_or_commentary` into `editorial`/`guideline`/`commentary`; no training notebook exists for this task since no labeled training data was available (a zero-shot prompt classifier is used directly).
- `thematic_classification/`
  - `theme_classifier_training.ipynb` — GEPA training of the theme classifiers.
  - `thematic_classification_{gemini,gpt,grok}.ipynb` — run theme classification with each model.
  - `validation_preprocessing.ipynb` — builds the 50-sample theme validation/ground-truth set.

### `results/`

Raw and ensembled outputs of the classifiers (JSON), one subfolder per classification stage.

- `classification/` — **stance** classification results (`Unrelated`/`Animal Study`/`Neutral`/`Supports PTLDS`/`Supports CLD`) over the whole corpus:
  - `stuedent_..._gpt-5-mini...json`, `stuedent_..._gemini-2.0-flash-001...json`, `stuedent_..._grok-4-fast...json` — per-model results, each item with `abstract`, `predicted_decision`, `predicted_reasoning`, `predicted_confidence`, `ground_truth_decision` (where available) and `predicition_correct`.
  - `ensembled_classification_results.json` — the three models' stance predictions combined (majority vote) into one `predicted_decision` per abstract.
  - `ensembled_classification_results_detailed_filtered.json` — the ensembled results filtered down to only the abstracts relevant to the debate (dropping `Unrelated`/`Animal Study`).
  - `items_without_themes.json` — abstracts classified as relevant (stance) but that ended up with no assigned theme after thematic classification (12 items) — useful for spotting gaps/failures in the thematic stage.
- `domain_classification/` — **study-design** classification results (`primary_design`/`secondary_designs`) plus the **editorial** follow-up step:
  - `classification_results_{gemini,gpt,grok}_original.json` — per-model study-design classification (`abstract`, `primary_design`, `secondary_designs`), taxonomy including `randomized_controlled_trial`, `case_series`, `case_report`, `systematic_review_or_meta_analysis`, `animal_model`, `in_vitro`, `diagnostic_accuracy_study`, `guideline_or_editorial_or_commentary`, etc.
  - `ensembled_classification_results_original.json` — ensembled study-design classification across the three models.
  - `editorial_classification_results_{gemini,gpt,grok}_original.json` — per-model classification of the abstracts flagged `guideline_or_editorial_or_commentary`, each item labeled `editorial`, `commentary`, or `guideline` (`abstract`, `author`, `label`).
  - `ensembled_editorial_classification_results_original.json` — ensembled editorial classification.
- `thematic_classification/` — **thematic** classification results (one or more of the 7 debate themes per abstract):
  - `three_theme_classification_results_{gemini,gpt}_original.json`, `three_theme_classification_results_gpt_original_gepa_reflacor_gpt-4omini.json`, `three_theme_classification_results_grok_original.json` — per-model theme classification results.
  - `three_theme_classification_results_ensembled_three_majority_vote.json` — final theme labels via majority vote across the three models; each item has `themes` (the winning theme list) and `themes_count` (`[theme, vote count]` pairs showing how many models assigned each theme).
- `final_classification_results.json` — the final, merged per-abstract dataset (8,630 entries) combining bibliographic metadata (`Title`, `Publication`, `Doi`, `Authors`, `Year`, `Type`) with the study design (`primary_design`, `secondary_designs` — with the `guideline_or_editorial_or_commentary` bucket already resolved into `editorial`/`commentary`/`guideline`), assigned `themes`, and the final stance label (`odds`). This is the end product of the whole pipeline.

### `validation/`

Human-annotated ground truth used to evaluate the classifiers and measure inter-rater agreement. Unless noted otherwise, these are for the **stance** classification (`Unrelated`/`Animal Study`/`Neutral`/`Supports PTLDS`/`Supports CLD`).

- `train_groundtruth.csv` (54 rows), `val_groundtruth.csv` (18 rows), `test_groundtruth.csv` (19 rows) — stance train/validation/test splits, each balanced across the 5 stance classes, with the original and revised human classification (`revised_classification`, `revised_confidence`, `revised_reason`) plus two independent raters' labels (`interrater_1_*`, `interrater_2_*`).
- `interrater_samples_complete.csv` (150 rows, 30 per stance class) — the full inter-rater reliability set: the two human raters' stance labels alongside classifications and reasons from every model/configuration tried (`gemini`, `gemini_thinking`, `claude`, `deepthink`, `qwen`, `qwen_v2`, `grok_3`), plus agreement flags (`full_agreement_classification`, `interrater_agreement_classification`, etc.) — used to measure human-human and human-model agreement.
- `domain_classification/` — ground truth for the **study-design** classification (`primary_design`/`secondary_designs`), separate from the stance ground truth above:
  - `classification_results_ensembled_domain_classification.json` and its `_train`/`_val`/`_test` variants — ensembled study-design classification over the corresponding split.

