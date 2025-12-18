![GitHub last commit](https://img.shields.io/github/last-commit/lira1013/PBMF_simulation-note)
![GitHub repo size](https://img.shields.io/github/repo-size/lira1013/PBMF_simulation-note)
# PBMF Simulation Note  

&gt; Plain-language Q&A + full analysis report (Word→Markdown) for the *Cancer Cell* virtual-patient generator.  

## 📄 Files  
1. `Q&A_ABOUT_BASIC_KNOWLEDGE_FOR_ME.md` – concise explanations of contrastive learning, distillation, covariance matrix, agent applicability, etc.  
2. `Simulate_Patient_Generation_and_PBMF_Model_Application_Analysis_Report.md` – complete reproduction & clinical-viability assessment (tables, figures, optimization directions).  

## 🔑 Take-home Messages  
- Contrastive learning in PBMF compares **survival of B+/B- across arms** → selects *predictive* markers only.  
- Virtual patients use **Weibull + identity covariance** → controllable, noise-robust, 1:1 grouping for fair AUPRC.  
- PBMF outperforms VT & SIDES on **non-linear combinatorial signals** (AUPRC 0.918 ± 0.047).  
- Cox is *not* the generator—only used for **HR validation** (B+ HR ≈ 0.50, p &lt; 0.05).  
- Upgrade path: identity → real covariance matrix for **clinical-grade agent** integration.  

## 📬 Cite This  
```bibtex
@misc{pbmf_sim_note,
  title={PBMF Simulation Note},
  author={Your Name},
  year={2025},
  publisher={GitHub},
  url={https://github.com/lira1013/PBMF_simulation-note}
}
This repository contains reading notes and an independent analysis of the PBMF framework published in Cancer Cell (Arango-Argoty et al., 2025).
The original code is available under the MIT License at https://doi.org/10.5281/zenodo.14766044.
