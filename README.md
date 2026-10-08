# ASD Blood Transcriptomic ML

A machine learning and bioinformatics project for identifying robust candidate transcriptomic signatures associated with autism spectrum disorder using peripheral blood gene expression data.

## Research Question

Can robust peripheral-blood transcriptomic features associated with autism spectrum disorder be identified, and can appropriately validated machine-learning ensembles improve ASD classification while maintaining generalizability and avoiding overfitting?

## Dataset

This project uses publicly available gene expression data from the Gene Expression Omnibus (GEO).

- GEO accession: GSE18123
- Platform: GPL6244
- Tissue: Peripheral blood
- Disease: Autism Spectrum Disorder

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