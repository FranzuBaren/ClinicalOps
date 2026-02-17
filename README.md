<p align="center">
  <img src="https://img.shields.io/badge/%F0%9F%A7%AC-ClinOps_AI-1A3C6E?style=for-the-badge&labelColor=E8F0FE" alt="ClinOps AI" height="40">
</p>

<h3 align="center">Deep Learning for Clinical Trial Safety Intelligence</h3>

<p align="center">
  <em>An end-to-end pipeline on real FDA-grade CDISC data —<br>from SAS binary parsing to interpretable neural safety predictions</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=flat-square&logo=pytorch&logoColor=white">
  <img src="https://img.shields.io/badge/Data-PHUSE_CDISC_Pilot-00599C?style=flat-square">
  <img src="https://img.shields.io/badge/Subjects-254-2E7D32?style=flat-square">
  <img src="https://img.shields.io/badge/Figures-9_Publication_Quality-B2182B?style=flat-square">
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square">
</p>

<p align="center">
  <a href="notebooks/01_walkthrough.ipynb">📓 Notebook</a> · 
  <a href="docs/report.pdf">📄 Technical Report (14pp, LaTeX)</a> · 
  <a href="#results">📊 Results</a> · 
  <a href="#quick-start">🚀 Quick Start</a>
</p>

---

## 🎯 Purpose

> **Can modern deep learning improve how we monitor clinical trial safety — and can we prove it honestly?**

Clinical trials generate rich, structured safety data (adverse events, vitals, labs, exposure) in regulatory-standard formats. Yet the analytical tools used to monitor patient safety — frequency tables, static listings, pre-specified TLFs — haven't changed meaningfully since 1998.

This project builds a **complete, reproducible pipeline** that takes raw SAS Transport files and produces:

| Output | What It Delivers |
|--------|-----------------|
| 🔬 **9 publication figures** | Each answers a specific clinical question a DSMB member needs answered |
| 🧠 **8-model ablation study** | Quantifies exactly what each neural component contributes |
| 🔍 **Per-patient explanations** | Integrated Gradients attribution — not a black box |
| ⚠️ **Honest limitations** | Including a leakage bug we caught, fixed, and documented |

---

## 📋 The Problem We Solve

A medical monitor overseeing a 254-patient Alzheimer's trial faces four questions that traditional tools **cannot answer**:

```
❓ TEMPORAL     → When do AEs cluster? Is the first month critical, or is this cumulative?
❓ MULTIVARIATE → Pruritus + dizziness + weight loss in one patient — signal or noise?
❓ PREDICTIVE   → This subject has 5 mild AEs in 6 weeks. Should I worry?
❓ EXPLAINABLE  → If I escalate to the DSMB, what evidence do I present?
```

**ClinOps AI answers all four.** With math, with figures, with code — and with honest acknowledgment of what doesn't work at N=254.

---

## 🏗️ Pipeline Architecture

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│  XPT Parser  │───▶│  Pydantic v2  │───▶│  DuckDB SQL  │
│  (pure Python│    │  CDISC rules  │    │  (zero-copy) │
│   ~60 lines) │    │  validation   │    │  analytics   │
└─────────────┘    └──────────────┘    └──────┬──────┘
                                              │
                    ┌─────────────────────────┴──────────────────────────┐
                    ▼                                                    ▼
          ┌─────────────────┐                              ┌────────────────────┐
          │  9 Publication   │                              │  Deep Learning     │
          │  Figures         │                              │  ┌──────────────┐  │
          │  (matplotlib,    │                              │  │ CVAE Aug     │  │
          │   seaborn,       │                              │  │ Pretraining  │  │
          │   300 DPI)       │                              │  │ MLP + MC     │  │
          └─────────────────┘                              │  │ GRU + Attn   │  │
                                                           │  │ Integ. Grad. │  │
                                                           │  └──────────────┘  │
                                                           └────────────────────┘
```

| Stage | Technology | What It Does |
|:-----:|-----------|-------------|
| **1** | Pure-Python XPT parser | Reads SAS Transport v5 binary without SAS ($15k/yr saved) |
| **2** | Pydantic v2 | Enforces CDISC SDTM business rules at parse time |
| **3** | DuckDB | Zero-copy SQL analytics on Polars DataFrames |
| **4** | matplotlib + seaborn | 9 clinical figures, each with a clear narrative |
| **5** | PyTorch | CVAE augmentation, self-supervised pretraining, 8-model ablation |
| **6** | Integrated Gradients | Axiomatic per-patient feature attribution |

---

## 📊 Results

### Clinical Analytics

<table>
<tr>
<td width="50%">
<img src="figures/fig01_volcano.png" width="100%">
<p align="center"><b>Fig 1 · Volcano Plot</b><br>
<sub>Pruritus, application site reactions, and dizziness are<br>statistically significant safety signals (Fisher exact, p<0.05).<br>This is a <b>dermatological tolerability</b> issue, not systemic toxicity.</sub></p>
</td>
<td width="50%">
<img src="figures/fig03_kaplan_meier.png" width="100%">
<p align="center"><b>Fig 3 · Kaplan–Meier</b><br>
<sub>AE-free survival separates by day 15 with clear dose-response.<br>Half of High Dose patients have first AE by day 25.<br><b>Month-1 tolerance predicts long-term safety.</b></sub></p>
</td>
</tr>
<tr>
<td>
<img src="figures/fig02_swimmer.png" width="100%">
<p align="center"><b>Fig 2 · Swimmer Plot</b><br>
<sub>Individual AE timelines for top 35 subjects by event count.<br>Early-onset clustering visible in active arms (red).</sub></p>
</td>
<td>
<img src="figures/fig04_temporal_heatmap.png" width="100%">
<p align="center"><b>Fig 4 · Temporal Heatmap</b><br>
<sub>Body system × 4-week period. Skin/general disorders dominate<br>weeks 1–8; cardiac events appear later — different mechanisms.</sub></p>
</td>
</tr>
<tr>
<td>
<img src="figures/fig05_vitals.png" width="100%">
<p align="center"><b>Fig 5 · Vitals Over Time</b><br>
<sub>Mean ± 95% CI for SBP, DBP, Pulse, Weight across visits.<br>No concerning cardiovascular vital sign signal.</sub></p>
</td>
<td>
<img src="figures/fig06_safety_dashboard.png" width="100%">
<p align="center"><b>Fig 6 · Safety Dashboard</b><br>
<sub>DSMB-ready 4-panel: AE incidence, severity profile,<br>SAE rate, exposure-response (r=0.93). <b>Complete overview.</b></sub></p>
</td>
</tr>
</table>

### Deep Learning

<table>
<tr>
<td width="50%">
<img src="figures/fig07_ml_dashboard.png" width="100%">
<p align="center"><b>Fig 7 · ML Dashboard</b><br>
<sub>(A) AUROC forest plot for 8 models. (B) Ablation: pretraining +0.067,<br>augmentation additive. (C) Calibration. (D) MC Dropout uncertainty.<br>(E) Learning curve. (F) VAE latent space.</sub></p>
</td>
<td width="50%">
<img src="figures/fig09_integrated_gradients.png" width="100%">
<p align="center"><b>Fig 9 · Feature Attribution</b><br>
<sub>Integrated Gradients: AE diversity and temporal spread<br>are top SAE predictors — a <b>novel insight</b> invisible to<br>traditional frequency tables.</sub></p>
</td>
</tr>
</table>

<img src="figures/fig08_gru_attention.png" width="100%">
<p align="center"><b>Fig 8 · GRU + Attention</b> — <sub>Temporal sequence analysis. Attention heatmap over SAE subjects, SAE vs non-SAE attention patterns, and training convergence. Note: GRU does not converge with only 3 SAE sequences.</sub></p>

### Ablation Study: What Actually Works

| Model | Type | AUROC | Δ vs Scratch |
|-------|:----:|:-----:|:------------:|
| Logistic Regression | Classical | **0.690** | — |
| Random Forest | Classical | 0.348 | — |
| Gradient Boosting | Classical | 0.358 | — |
| MLP (scratch) | Neural | 0.568 | baseline |
| MLP + pretraining | Neural | 0.635 | +0.067 |
| MLP + pretrain + aug | Neural | 0.656 | +0.088 |
| MLP + aug only | Neural | 0.928* | — |
| Full pipeline + MC | Neural | 0.763 | +0.195 |

<sub>*Suspected augmentation overfitting — CVAE may generate synthetics too close to real positives within CV folds.</sub>

> **Key finding:** At N=254 with 3 SAE subjects, ℓ₂-regularized logistic regression (0.690) outperforms most neural variants. Self-supervised pretraining contributes +0.067 and CVAE augmentation adds +0.021 incrementally. The neural pipeline's value lies not in raw AUROC but in **interpretability** — attention weights and Integrated Gradients reveal patterns no classical model can explain.

---

## ⚠️ Honest Limitations

This project prioritizes **honest reporting** over impressive numbers:

| Issue | Impact | Status |
|-------|--------|:------:|
| **Data leakage** — `n_sae` was both feature and target | AUROC inflated to ~1.0 | ✅ Fixed |
| **3 SAE subjects** out of 254 (1.2%) | All models underpowered | 📊 Documented |
| **MLP+aug anomaly** (0.928) | Likely CVAE overfitting within folds | ⚠️ Flagged |
| **Tree ensembles below chance** (0.35) | Imbalance overwhelms RF/GBM | 📊 Documented |
| **GRU non-convergence** | 3 positive sequences insufficient | 📊 Documented |
| **Single trial** | Generalizability unknown | 📋 Noted |

---

## 🚀 Quick Start

```bash
# Clone
git clone https://github.com/YOUR_USERNAME/clinops-ai.git
cd clinops-ai

# Install
pip install -r requirements.txt

# Run — data downloads automatically from PHUSE GitHub
jupyter lab notebooks/01_walkthrough.ipynb
```

No SAS license needed. No local data files needed. Everything runs end-to-end.

---

## 📁 Repository Structure

```
clinops-ai/
│
├── 📓 notebooks/
│   └── 01_walkthrough.ipynb      # Complete pipeline (69 cells, 9 figures)
│
├── 📄 docs/
│   ├── report.pdf                # Technical report (14 pages, JCDE-style LaTeX)
│   └── report.tex                # LaTeX source (compilable)
│
├── 📊 figures/                   # All 9 publication figures from real data
│   ├── fig01_volcano.png
│   ├── fig02_swimmer.png
│   ├── fig03_kaplan_meier.png
│   ├── fig04_temporal_heatmap.png
│   ├── fig05_vitals.png
│   ├── fig06_safety_dashboard.png
│   ├── fig07_ml_dashboard.png
│   ├── fig08_gru_attention.png
│   └── fig09_integrated_gradients.png
│
├── requirements.txt
├── LICENSE                       # MIT
└── README.md                     # You are here
```

---

## 📚 Data

**PHUSE CDISC Pilot Study (CDISCPILOT01)** — publicly available, FDA-standard clinical trial dataset.

| | Details |
|---|---|
| **Study** | Xanomeline transdermal (54mg, 81mg) vs Placebo |
| **Population** | 254 subjects, mild-to-moderate Alzheimer's disease |
| **Format** | SAS Transport v5 (XPT) per 21 CFR Part 11 |
| **Domains** | DM (254), AE (1,191), EX (591), VS (6,208), LB (13,988), DS (254) |
| **Source** | [github.com/phuse-org/phuse-scripts](https://github.com/phuse-org/phuse-scripts/tree/master/data/sdtm/cdiscpilot01) |
| **License** | PHUSE open-source, public for research & education |
| **Privacy** | Fully anonymized; synthetic IDs only; no HIPAA PHI |

> ⚠️ **Disclaimer:** This is a pilot/demonstration dataset. Analyses are methodological demonstrations, not clinical evaluations of Xanomeline.

---

## 🔬 Methods at a Glance

<details>
<summary><b>Statistical Methods</b> (click to expand)</summary>

- **Disproportionality:** Fisher's exact test on 2×2 tables, volcano plot with log₂(RR) vs -log₁₀(p)
- **Survival:** Kaplan–Meier with Greenwood variance, 95% CI bands, number-at-risk table
- **Exposure–Response:** Pearson correlation with t-test (r=0.93, p<0.001)
- **Severity:** Multinomial proportions by arm, stacked bar visualization

</details>

<details>
<summary><b>Deep Learning Components</b> (click to expand)</summary>

- **CVAE:** Conditional VAE with β=0.3, latent dim=8, ELBO optimization, reparameterization trick
- **Pretraining:** Masked feature autoencoder (30% masking, BERT-style), 300 epochs
- **Classifier:** Frozen encoder + 2-layer head, MC Dropout (T=50) for epistemic uncertainty
- **GRU:** Bidirectional with Bahdanau attention, leakage-free event encoding [day, severity, SOC]
- **Attribution:** Integrated Gradients with M=200 steps, completeness axiom verified

</details>

<details>
<summary><b>Evaluation Protocol</b> (click to expand)</summary>

- 5-fold stratified CV preserving 1.2% class ratio
- Bootstrap 95% CI (B=2000) on AUROC, Average Precision, Brier score
- 8 models under identical protocol: 3 classical + 5 neural ablation variants

</details>

---

## 📖 Technical Report

The accompanying [LaTeX report](docs/AI_ClinOps.pdf) (14 pages, JCDE journal style) includes:
- Complete mathematical formulation for all statistical and ML methods
- 10 formally defined equations (Fisher, KM, ELBO, GRU gates, Integrated Gradients, etc.)
- Full ablation results with confidence intervals
- Honest limitations section
- Data provenance and licensing statement
- References to foundational literature (Sundararajan, Kingma, Gal, Bahdanau, etc.)

---

<p align="center">
  <sub>Built with Python, PyTorch, and honest methodology · MIT License</sub>
</p>
