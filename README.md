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

## Data Preprocessing & Curation Pipeline

The initial pipeline transforms the raw microarray data (`GSE18123_family.soft`) into a clean, leakage-free feature matrix ready for statistical genomics and machine learning workflows. This process is divided into four distinct phases:

---

### 1. Platform Subsetting & Cohort Selection
- **Cross-Platform Batch Effect Removal:** The complete GSE18123 series encompasses 285 samples profiled across two distinct microarray platforms (`GPL570` [$n = 99$] and `GPL6244` [$n = 186$]). To prevent confounding technical batch artifacts introduced by divergent chip chemistries and probe designs, the analysis strictly isolates the **186 samples profiled on the Affymetrix Human Gene 1.0 ST platform (`GPL6244`)**.
- **Cohort Alignment:** All subsequent clinical metadata curation and expression parsing are locked to these 186 peripheral whole-blood samples.

---

### 2. Clinical Metadata Curation & Target Encoding
- **Structured Attribute Parsing:** Unstructured clinical strings stored within `characteristics_ch1` (age, diagnosis, gender, ethnicity, etc.) were parsed into key-value pairs and structured into a tabular DataFrame (`metadata_clean`).
- **Binary Target Formulation:** Clinical diagnostic classifications were consolidated into a binary phenotype according to standardized psychiatric and DSM criteria:
  - **Positive Class (`ASD`, label = 1):** $n = 104$, consisting of:
    - Pervasive Developmental Disorder Not Otherwise Specified (PDD-NOS): $n = 48$
    - Classic Autism: $n = 41$
    - Asperger's Disorder: $n = 15$
  - **Negative Class (`Control`, label = 0):** $n = 82$ (neurotypical control subjects).
- **Confounder Profiling:**
  - **Age:** Well-balanced across cohorts (ASD mean = $97.1 \pm 48.6$ months; Control mean = $96.4 \pm 57.2$ months; missing values = $0$).
  - **Gender:** A marked imbalance exists (ASD M:F ratio $\approx 3.33:1$ vs. Control M:F ratio $\approx 1.41:1$). This covariate is tracked and controlled during downstream statistical modeling to avoid spurious gender-driven discoveries.

---

### 3. Probe Expression Matrix Extraction & Scale Normalization
- **Matrix Assembly:** Individual sample expression profiles (`ID_REF` vs. `VALUE`) were merged by probe ID to construct a unified probe-level intensity matrix of size **$33,297 \text{ probes} \times 186 \text{ samples}$**.
- **Sanity Audits:**
  - Total missing values: **0** (100% complete matrix).
  - Duplicate probe identifiers: **0** (strictly unique index).
- **Variance Stabilization:** Upstream data processing utilized the Affymetrix PLIER (Probe Logarithmic Intensity Error) algorithm, generating raw intensities on a linear scale characterized by severe right-skewness (sample means $\sim 330\text{--}370$, peaks $> 21,000$). To stabilize variance and achieve approximate normality, a logarithmic transformation was applied:
  $$\text{Expression}_{\log_2} = \log_2(\text{Intensity} + 1.0)$$

---

### 4. Probe-to-Gene Mapping & Redundancy Aggregation
- **Platform Annotation Mapping:** The `GPL6244` annotation metadata table was queried to map manufacturer probe set identifiers (`ID_REF`) to official HUGO Gene Nomenclature Committee (HGNC) **Gene Symbols** parsed from the `gene_assignment` field.
- **Unmapped Probe Filtering:** Control probes, non-hybridizing sequences, and unannotated genomic regions were removed, yielding **25,293 high-confidence annotated probes**.
- **Probe Redundancy Resolution (Median Aggregation):** For multi-probe genes targeting identical transcripts, signals were consolidated by calculating the **median expression** across corresponding probes, mitigating probe-level outlier effects.
- **Final Modeling Tensors:** The consolidated matrix was transposed to conform to canonical machine learning conventions (rows = instances, columns = features):
  - **Feature Matrix ($X$):** $186 \text{ samples} \times 23,307 \text{ unique genes}$
  - **Target Vector ($y$):** $186 \text{ class labels}$ (Control: $82$, ASD: $104$)

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