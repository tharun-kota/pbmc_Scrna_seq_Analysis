# Single-Cell RNA-seq Analysis of PBMCs

This repository contains the analysis workflow and results from a **single-cell RNA-seq study of human peripheral blood mononuclear cells (PBMCs)**. The pipeline integrates **quality control (QC), dimensionality reduction, clustering, marker-based annotation, and intercellular communication analysis** using [Seurat](https://satijalab.org/seurat/) and [NicheNet](https://github.com/saeyslab/nichenetr).

---

## 📌 Analysis Workflow

### 1. Quality Control (QC)
- Cells with **<200 detected genes** or **>2,500 genes** were removed to eliminate low-quality or multiplet cells.  
- Cells with **>5% mitochondrial gene content** were excluded, as these often represent stressed or dying cells.  
- QC plots (violin plots, scatterplots) were generated to visualize filtering thresholds.

### 2. Normalization & Feature Selection
- Data was normalized using **Seurat’s log-normalization** method.  
- Highly variable genes (HVGs) were identified to focus downstream analysis on informative features.

### 3. Dimensionality Reduction
- **PCA** was applied to HVGs for linear dimensionality reduction.  
- The **ElbowPlot** was used to determine the optimal number of PCs to retain.  
- **UMAP** embedding was generated using these PCs to visualize the global structure of immune cell heterogeneity.

### 4. Clustering & Cell Type Annotation
- Shared nearest neighbor (SNN) clustering was performed to group cells.  
- Clusters were annotated into immune subsets using canonical marker genes:  
  - **CD14⁺ Monocytes:** CD14, LYZ  
  - **Memory CD4⁺ T cells:** IL7R, CCR7, S100A4  
  - **Other subsets:** NK cells, dendritic cells, B cells, platelets  

### 5. Intercellular Communication (NicheNet)
- Sender cells: **CD14⁺ monocytes**  
- Receiver cells: **Memory CD4⁺ T cells**  
- Differential expression in T cells defined **target genes**.  
- **Candidate ligands** were identified from monocytes and matched to receptors on T cells.  
- Ligands were ranked based on their ability to predict T-cell target gene expression.  
- Key predicted interactions:  
  - **TNF → TNFRSF1A/B** (pro-inflammatory signaling)  
  - **CXCL9 → CXCR3** (chemotaxis)  
  - **IL2 → IL2RB** (T cell proliferation)  
  - **SIRPG → CD47/CD5/CD9** (co-stimulatory axis)  

### 6. Visualization
- Heatmaps of ligand–target interactions  
- Chord diagrams linking ligands to predicted targets  
- UMAP plots colored by clusters and marker expression  

---

## 📊 Key Findings
- CD14⁺ monocytes regulate memory CD4⁺ T cells via a combination of **inflammatory (TNF, IFNG), chemotactic (CXCL9), and co-stimulatory (SIRPG–CD47)** signals.  
- This suggests a layered regulation of T cell function, with implications for immune activation and tolerance.  

---


---

## 💻 Requirements

This analysis was conducted in **R (≥4.2)** with the following key packages:

- [Seurat](https://satijalab.org/seurat/) (single-cell analysis)
- [NicheNet](https://github.com/saeyslab/nichenetr) (ligand–target modeling)
- ggplot2, dplyr, patchwork, pheatmap

Install with:

```r
install.packages(c("ggplot2", "dplyr", "patchwork", "pheatmap"))
if (!requireNamespace("Seurat", quietly = TRUE)) {
    remotes::install_github("satijalab/seurat")
}
if (!requireNamespace("nichenetr", quietly = TRUE)) {
    remotes::install_github("saeyslab/nichenetr")
}
```

