# ASD Blood Transcriptomic ML

A machine learning and bioinformatics project for identifying robust candidate transcriptomic signatures associated with autism spectrum disorder using peripheral blood gene expression data.

## Research Question

Can robust peripheral-blood transcriptomic features associated with autism spectrum disorder be identified, and can appropriately validated machine-learning ensembles improve ASD classification while maintaining generalizability and avoiding overfitting?

## Dataset Overview

### General Information
- **Accession ID:** [GEO: GSE18123](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE18123)
- **Source File:** `GSE18123_family.soft`
- **Total Samples in Series:** 285 samples profiled across two microarray platforms (`GPL570`: 99 samples, `GPL6244`: 186 samples).
- **Selected Platform:** **`GPL6244`** (Affymetrix GeneChip Human Gene 1.0 ST Array), focusing on all 186 samples to eliminate cross-platform batch effects.
- **Tissue & Molecule:** Whole peripheral blood; total RNA transcriptomic profiling.
- **Upstream Preprocessing:** Raw data preprocessed using the Probe Logarithmic Intensity Error (PLIER) estimation method (Affymetrix Power Tools v1.10).

---

### Cohort Demographics & Clinical Characteristics

#### Diagnostic Subtypes & Binary Classification
The original cohort includes four primary diagnostic categories, consolidated into a binary classification scheme for downstream machine learning tasks:
- **Autism Spectrum Disorder (ASD)**: **104 samples** (1 = Positive Class)
  - *PDD-NOS:* 48
  - *Autism:* 41
  - *Asperger's Disorder:* 15
- **Control Group**: **82 samples** (0 = Negative Class)
- **Total Cohort Size:** **186 samples**

#### Gender Distribution
- **Overall:** 128 males / 58 females
- **ASD Group:** 80 males / 24 females (M:F ratio ≈ 3.33:1)
- **Control Group:** 48 males / 34 females (M:F ratio ≈ 1.41:1)
> *Note:* A statistically significant gender imbalance exists between groups, necessitating careful evaluation of potential confounding effects.

#### Age Distribution (Months)
- **Missing Values:** None (0% missing)
- **Overall Cohort:** Mean = 96.8 ± 52.4 months (Range: 24 – 264 months)
- **ASD vs. Control:**
  - *ASD:* Mean = 97.1 ± 48.6 months
  - *Control:* Mean = 96.4 ± 57.2 months
> *Note:* Age distributions are well-balanced across both diagnostic classes.

---

### Expression Matrix Summary (Pre-Annotation)

- **Dimensions:** 33,297 probe IDs × 186 samples
- **Missing Values:** 0 (Complete data matrix)
- **Duplicate Probe IDs:** 0 (Unique index)
- **Signal Scale:** Raw/linear intensity scale (mean per-sample intensity: ~330–370, maximum intensity > 21,000). Transformed to $\log_2(\text{intensity} + 1)$ to normalize positive skewness prior to feature selection and classification.

## Project Goals

1. Preprocess and quality-control peripheral blood transcriptomic data.
2. Identify robust and reproducible candidate transcriptomic features.
3. Evaluate multiple machine-learning models.
4. Investigate whether ensemble learning improves classification.
5. Validate the final findings on an independent dataset when possible.
6. Investigate the biological relevance of the identified features.

## Project Structure

```text
data/          Data and metadata
notebooks/     Exploratory and analysis notebooks
src/           Reusable Python functions
results/       Figures and tables