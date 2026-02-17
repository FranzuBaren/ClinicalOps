# Contributing to ClinOps AI

Thank you for your interest in contributing! This project welcomes contributions that improve the analytical pipeline, fix bugs, enhance documentation, or extend the methodology.

## How to Contribute

### Reporting Issues

- Use [GitHub Issues](../../issues) to report bugs or suggest enhancements
- Include the Python version, OS, and a minimal reproducible example
- For data-related issues, specify which SDTM domain and cell number in the notebook

### Pull Requests

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Ensure the notebook `AI_ClinOps.ipynb` runs end-to-end without errors
5. Submit a pull request with a clear description

### Development Setup

```bash
git clone https://github.com/YOUR_USERNAME/clinicalops.git
cd clinicalops
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab AI_ClinOps.ipynb
```

### Code Style

- Python code follows PEP 8
- Notebook cells should be self-contained and well-commented
- New figures should follow the existing design language (serif fonts, clinical palette, annotation-driven)

### Areas Where Help Is Welcome

- **External validation:** Applying the pipeline to other CDISC datasets
- **Augmentation investigation:** Diagnosing the MLP+aug AUROC=0.928 anomaly
- **Scaling:** Adapting the pipeline for FAERS or multi-trial pooled data
- **TDA integration:** Persistent homology on the clinical feature space
- **Visualization:** Additional clinical figure types (forest plots, funnel plots)

## Code of Conduct

Please read [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) before contributing.
