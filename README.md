# Mini Project 1 — Xenopus Regeneration Organizing Center (ROC)

This repository reproduces the identification of the **Regeneration Organizing Center (ROC)** in *Xenopus laevis* tail regeneration using single-cell RNA sequencing (scRNA-seq) data.  
The analysis follows the workflow described in *Aztekin et al., Nature 2019* (Figure 1B, Skin panel).  
Multiple clustering and marker-selection methods are used to isolate the ROC population and compare its defining genes with those reported in Supplementary Table 3 of the paper.

---

## Overview

The project aims to  
1. Reproduce clustering of the *Xenopus* tail single-cell transcriptomic dataset.  
2. Identify the ROC cluster and determine which genes distinguish it from other cell populations.  
3. Compare the identified ROC markers with the reference gene list from Supplementary Table 3.  
4. Evaluate the impact of denoising and batch-integration methods on clustering quality and marker robustness.

---

## Analysis Workflow

### 1. Data Loading and Preprocessing
- Dataset: `ArrayExpress E-MTAB-7716`  
- Input: `countsMatrix.mtx`, `genes.csv`, `cells.csv`, `labels.csv`, `meta.csv`  
- Steps performed in **Scanpy**:
  - Filtered low-quality cells and genes (`min_genes=200`, `min_cells=3`)
  - Normalized total counts (`sc.pp.normalize_total`)
  - Log-transformed data (`sc.pp.log1p`)
  - Selected 3,000 highly variable genes (HVGs)
  - Reduced dimensionality with PCA  

### 2. Clustering Analysis
- Built the neighborhood graph (`sc.pp.neighbors`)  
- Visualized data with UMAP (`sc.tl.umap`)  
- Applied three clustering algorithms:
  - **Leiden** and **Louvain** (graph-based)
  - **K-Means** (distance-based)
- Computed clustering metrics:
  - Silhouette score (cluster compactness)
  - Adjusted Rand Index (ARI)
  - Normalized Mutual Information (NMI)
- The PCA + Leiden and PCA + Louvain analyses fulfill the “two clustering algorithms” requirement.

### 3. Marker Selection and ROC Identification
- Marker detection using two methods:
  - **Wilcoxon rank-sum test**
  - **Logistic regression**
- Compared ROC marker lists across methods.
- Measured overlap with the canonical ROC reference list from Supplementary Table 3.
- Key visualizations:
  - Dot plot showing ROC gene expression across clusters
  - UMAP highlighting the ROC cluster in color (other clusters in gray)

### 4. Denoising and Robustness
- Denoising algorithms:
  - **MAGIC** (graph-based imputation)
  - **scVI** (variational autoencoder)
- Evaluated each method using Silhouette, ARI, NMI and ROC-gene overlap under:
  - Different Leiden resolutions (0.6, 1.0, 1.2)
  - 80 % random subsampling
- ROC remained stable across all variants.

### 5. Batch Integration
- Integration algorithms:
  - **Harmony**
  - **BBKNN**
- Assessed the effect of integration on:
  - Clustering performance (Silhouette, ARI, NMI)
  - ROC marker consistency
- Integration preserved the ROC cluster and its marker profile.

---

## Output Files

| Directory | Contents |
|------------|-----------|
| `figures/` | UMAPs, dot plots, denoising and integration visualizations |
| `outputs/` | Metrics tables, overlap summaries, robustness results |
| `refs/` | ROC reference gene list (`rocGenes.csv`) |

**Key files**
- `metrics_baseline.csv` — baseline clustering metrics  
- `robustness.csv` — ROC overlap across resolutions/subsampling  
- `metrics_all_variants.csv` — all clustering and integration metrics  
- `summary_results.csv` — combined summary table  
- `ROC_markers_wilcox_logreg.csv` — ROC markers shared across methods  

---

## How to Run (Google Colab)

1. Mount Google Drive and open the project folder.  
2. Open **`Frog_and_tail_FINAL_Dasha_Lykova_dl3415.ipynb`**.  
3. Run cells sequentially (A–J for main analysis, K–N for optional denoising/integration).  
4. Figures are saved to `figures/`, metrics to `outputs/`.

---

## Code Availability

- **Colab notebook:** *insert Colab link here*  
- **GitHub repository:** *insert GitHub link here*

---

## Environment and Requirements

Main packages and versions:

scanpy==1.10.2
anndata==0.12.2
scvi-tools==1.1.2
torch (CPU build)
magic-impute
harmonypy
bbknn
scanorama
igraph, leidenalg, louvain
pandas==2.2.2
numpy==2.0.2
scipy
scikit-learn
umap-learn
matplotlib
seaborn


---

## Summary

This pipeline isolates the ROC population (Leiden cluster 18) characterized by strong expression of canonical ROC genes such as *EGFL6*, *FREM2*, *PLTP*, *NID2*, and *IGFBP2*.  
Comparison with Supplementary Table 3 confirms substantial overlap between the identified markers and the published ROC gene set.  
Clustering, denoising, and integration analyses all show that the ROC cluster remains distinct and biologically coherent across computational methods.

---

**Author:** Dasha Lykova  
**Institution:** Columbia University — Data Science & Financial Economics  
**Course:** Introduction to Data Science — Mini Project 1  
**Date:** October 2025

