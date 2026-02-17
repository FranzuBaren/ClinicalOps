# Changelog

All notable changes to this project are documented in this file.

## [1.0.0] — 2026-02-17

### 🎉 Initial Release

**Pipeline (Stages 1–3)**
- Pure-Python SAS Transport v5 (XPT) parser — no SAS dependency
- Pydantic v2 models for CDISC SDTM domain validation (DM, AE, EX, VS, LB, DS)
- DuckDB zero-copy SQL analytics on Polars DataFrames

**Clinical Visualizations (Stage 4)**
- Fig 1: Volcano plot — AE disproportionality with Fisher exact test
- Fig 2: Swimmer plot — individual subject AE timelines (top 35)
- Fig 3: Kaplan–Meier — time-to-first-AE with 95% CI and risk table
- Fig 4: Temporal heatmap — body system × 4-week period event density
- Fig 5: Vitals — 4-panel longitudinal mean ± 95% CI
- Fig 6: Safety dashboard — DSMB-ready 4-panel composite
- Publication design language: serif fonts, clinical palette, annotation-driven

**Deep Learning (Stage 5)**
- Conditional VAE for minority class augmentation (β-ELBO)
- Self-supervised masked feature pretraining (BERT-style, 30% masking)
- MLP classifier with MC Dropout uncertainty (T=50 passes)
- Bidirectional GRU with Bahdanau self-attention (leakage-free encoding)
- 8-model ablation under 5-fold stratified CV with bootstrap 95% CI

**Interpretability (Stage 6)**
- Integrated Gradients with completeness axiom verification
- Per-subject beeswarm attribution plots

**Documentation**
- 14-page LaTeX technical report (JCDE journal style, 10 formal definitions)
- Comprehensive README with all 9 figures and ablation table
- Data provenance, licensing, and privacy statement

### 🐛 Bug Fixes
- **Critical:** Removed `n_sae` from feature set — was causing target leakage (AUROC=1.0 → 0.5–0.7)
- **Critical:** Replaced `n_ae` (all AEs) with `n_ae_nonsevere` to prevent partial leakage
- Fixed `GradientBoostingClassifier` and `RandomForestClassifier` positional args for scikit-learn 1.4+
- Fixed `train_nn()` keyword argument (`epochs` → `ep`)
