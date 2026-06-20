# Supervised Topological Data Analysis for MALDI-MSI

**Internship project · Datrix S.p.A., Milan · in collaboration with Istituto Nazionale dei Tumori (INT), Milan · 2023–2024**  
Reza Rajaee

---

## Overview

This project is a reimplementation and application of the supervised topological data analysis framework proposed by Klaila et al. (2023) for MALDI mass spectrometry imaging data. The framework extracts topological features from mass spectra using persistence transformation, then applies supervised classifiers to distinguish lung cancer subtypes.

The work was carried out in two phases: first, reproducing the original results on the publicly available lung cancer dataset used in the paper; second, applying the framework to a proprietary human lung tissue dataset provided by the Istituto Nazionale dei Tumori (INT), Milan.

---

## Reference paper

Klaila G, Vutov V, Stefanou A. *Supervised topological data analysis for MALDI mass spectrometry imaging applications.* BMC Bioinformatics, 24:279, 2023. [https://doi.org/10.1186/s12859-023-05402-0](https://doi.org/10.1186/s12859-023-05402-0)

Code and data from the original paper: [https://github.com/klailag/SupervisedTDAMethodForMALDI](https://github.com/klailag/SupervisedTDAMethodForMALDI)

---

## Datasets

**Dataset 1 — Lung cancer TMA (public)**
The publicly available MALDI-MSI dataset used in Klaila et al., containing 4,669 mass spectra from 304 non-small cell lung cancer patients (168 adenocarcinoma, 136 squamous cell carcinoma), organised in 8 tissue microarray blocks. Available via the original paper's repository.

**Dataset 2 — Human lung tissue (proprietary)**
A clinical MALDI-MSI dataset provided by the Istituto Nazionale dei Tumori (INT), Milan. Not publicly available.

---

## Methodology

The framework follows the two-step pipeline described in Klaila et al.:

### Step 1 — Topological feature extraction

Each mass spectrum is processed individually to extract its topological properties. The key concepts:

**Upper-level set filtration** — local maxima in the spectrum (peaks) are identified and tracked as topological features. Each feature has a birth value (the intensity of the peak) and a death value (the intensity at which the peak merges with a larger neighbouring peak).

**Persistence** — defined as the difference between birth and death values: p(x) = f(x) − μ(x). High persistence peaks correspond to strong, signal-carrying m/z values; low persistence peaks are more likely noise.

**Reduced persistence transformation** — for each peak x, the pair (position, persistence) is stored, compressing the original spectrum to a sparse representation containing only the most informative peaks. A tuning parameter k controls the fraction of peaks retained — by selecting only the top k% most persistent peaks, noise is filtered and data is compressed.

The algorithm for computing the persistence transformation runs in σ(q) + σ(m²) time, where q is the number of m/z values and m is the number of peaks per spectrum.

### Step 2 — Supervised classification

Classification was performed on the resulting persistence vectors using:

- **Logistic regression** — with logit link function, parameters estimated by maximum likelihood
- **Random forest** — 1,000 trees, √q candidate variables per split, Gini impurity splitting criterion

Both classifiers were evaluated using k-fold cross-validation at the TMA level (8-fold and 2-fold), following the experimental design of the original paper. Balanced accuracy was used as the evaluation metric to account for class imbalance.

---

## Implementation

The preprocessing and topological calculation were implemented in Python, combining:

- Adaptation of the original authors' published Python code
- Custom-built preprocessing functions for spectral data handling and peak processing
- scikit-learn for logistic regression and random forest classification

The implementation was then applied to the proprietary INT Milan dataset, requiring adaptation of the pipeline to the specific characteristics of that data.

---

## Key concepts studied

- Topological data analysis (TDA) and persistent homology
- Upper-level set filtration and the elder rule
- Persistence diagrams and persistence transformation
- Data compression via topological feature extraction
- Supervised classification on high-dimensional spectral data
- Cross-validation strategies for tissue microarray data

---

## Technical skills

| Area | Details |
|---|---|
| Topological data analysis | Persistent homology · upper-level set filtration · persistence transformation · merge trees |
| Signal processing | MALDI-MSI spectral data · peak detection · noise filtering via persistence |
| Machine learning | Logistic regression · random forest · k-fold cross-validation · balanced accuracy |
| Scientific Python | NumPy · scikit-learn · custom spectral preprocessing functions |
| Medical imaging | MALDI-MSI data structure · tissue microarray · lung cancer subtyping |

---

## References

- Klaila G, Vutov V, Stefanou A. *Supervised topological data analysis for MALDI mass spectrometry imaging applications.* BMC Bioinformatics, 24:279, 2023.
- Leuschner J et al. *Supervised non-negative matrix factorization methods for MALDI imaging applications.* Bioinformatics, 2019. — Comparison baseline
- Bemis KD et al. *Cardinal: an R package for statistical analysis of mass spectrometry-based imaging experiments.* Bioinformatics, 2015.
- Pedregosa F et al. *Scikit-learn: machine learning in Python.* JMLR, 2011.

---

## Repository note

This repository documents the methodology and implementation approach. Code and results are not publicly available due to a confidentiality agreement with Datrix S.p.A. The human lung tissue dataset is proprietary clinical data from INT Milan and is not publicly available.

---

*Internship project · Datrix S.p.A., Milan · 2023–2024 · Reza Rajaee*
