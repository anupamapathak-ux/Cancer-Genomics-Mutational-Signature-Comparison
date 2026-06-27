# Cancer Genomics: Mutational Signature Comparison Across Two Cancer Types Using Public TCGA Data

## Overview

This project was completed during a **7-week online internship at BioTechTrek** under the guidance of **Ms. H.S. Prithvi**. The project focuses on the comparative analysis of somatic mutations and mutational signatures in **Lung Adenocarcinoma (LUAD)** and **Colon Adenocarcinoma (COAD)** using publicly available **The Cancer Genome Atlas (TCGA)** datasets.

The study aims to identify differentially mutated genes, compare mutation frequencies, and investigate the biological processes responsible for cancer development using **maftools (R)** and **SigProfilerExtractor (Python)**.

---

## Objectives

- Download LUAD and COAD mutation datasets from TCGA.
- Analyze somatic mutations using the maftools package in R.
- Generate mutation summary plots and oncoplots.
- Compare mutation frequencies between LUAD and COAD.
- Extract mutational signatures using SigProfilerExtractor.
- Compare extracted signatures with COSMIC SBS signatures.
- Interpret the biological significance of the identified mutational processes.

---

## Software and Tools Used

- R
- Bioconductor
- maftools
- Python
- SigProfilerExtractor
- Windows PowerShell
- Microsoft Excel
- GitHub

---

## Dataset

**Source:** The Cancer Genome Atlas (TCGA)

**Cancer Types:**
- Lung Adenocarcinoma (LUAD)
- Colon Adenocarcinoma (COAD)

**Data Format:** Mutation Annotation Format (MAF)

---

# Workflow

## Step 1: Install Required Packages

```r
install.packages("BiocManager")
BiocManager::install("maftools")
library(maftools)
```

---

## Step 2: Load Mutation Data

```r
luad <- read.maf("LUAD.maf")
coad <- read.maf("COAD.maf")
```

---

## Step 3: Generate Mutation Summary

```r
plotmafSummary(
    maf = luad,
    rmOutlier = TRUE,
    addStat = "median",
    dashboard = TRUE,
    titvRaw = FALSE
)

plotmafSummary(
    maf = coad,
    rmOutlier = TRUE,
    addStat = "median",
    dashboard = TRUE,
    titvRaw = FALSE
)
```

---

## Step 4: Generate Oncoplots

```r
oncoplot(maf = luad, top = 10)

oncoplot(maf = coad, top = 10)
```

---

## Step 5: Export Gene Summary

```r
luad_gene_summary <- getGeneSummary(luad)
write.csv(luad_gene_summary,"LUAD_GeneSummary.csv",row.names=FALSE)

coad_gene_summary <- getGeneSummary(coad)
write.csv(coad_gene_summary,"COAD_GeneSummary.csv",row.names=FALSE)
```

---

## Step 6: Export Sample Summary

```r
luad_sample_summary <- getSampleSummary(luad)
write.csv(luad_sample_summary,"LUAD_SampleSummary.csv",row.names=FALSE)

coad_sample_summary <- getSampleSummary(coad)
write.csv(coad_sample_summary,"COAD_SampleSummary.csv",row.names=FALSE)
```

---

## Step 7: Compare LUAD and COAD

```r
comparison <- mafCompare(
    m1 = luad,
    m2 = coad,
    m1Name = "LUAD",
    m2Name = "COAD",
    minMut = 5
)
```

---

## Step 8: Forest Plot

```r
forestPlot(comparison)
```

---

## Step 9: Export Comparison Results

```r
write.csv(
    comparison$results,
    "LUAD_vs_COAD_Comparison.csv",
    row.names = FALSE
)
```

---

## Step 10: Top Differentially Mutated Genes

```r
top20 <- head(comparison$results,20)

write.csv(
    top20,
    "Top20_LUAD_COAD_Genes.csv",
    row.names = FALSE
)
```

---

## Step 11: Generate Bar Plot

```r
barplot(
    -log10(top20$adjPval),
    names.arg=top20$Hugo_Symbol,
    las=2,
    col="steelblue",
    main="Top Differentially Mutated Genes: LUAD vs COAD",
    ylab="-log10 Adjusted P-value"
)
```

---

## Step 12: Save Plot

```r
pdf("LUAD_vs_COAD_Top20Genes.pdf")

barplot(
    -log10(top20$adjPval),
    names.arg=top20$Hugo_Symbol,
    las=2,
    col="steelblue",
    main="Top Differentially Mutated Genes: LUAD vs COAD",
    ylab="-log10 Adjusted P-value"
)

dev.off()
```

---

## Step 13: Install SigProfilerExtractor

Run in **Windows PowerShell**:

```powershell
pip install SigProfilerExtractor
```

---

## Step 14: Import SigProfilerExtractor

```python
from SigProfilerExtractor import sigpro as sig
```

---

## Step 15: Extract Mutational Signatures

### LUAD

```python
sig.sigProfilerExtractor(
    "matrix",
    "LUAD_Signatures",
    "LUAD_Input"
)
```

### COAD

```python
sig.sigProfilerExtractor(
    "matrix",
    "COAD_Signatures",
    "COAD_Input"
)
```

---

## Results

The analysis revealed distinct mutational landscapes between LUAD and COAD. Mutation summary plots, oncoplots, and differential mutation analysis identified significant differences in mutation frequencies across several genes. SigProfilerExtractor identified characteristic SBS mutational signatures, with LUAD showing signatures associated with tobacco exposure and APOBEC activity, while COAD demonstrated signatures linked to mismatch repair deficiency, POLE mutations, and age-related mutational processes.

---

## Repository Contents

```
README.md
LUAD.maf
COAD.maf
LUAD_GeneSummary.csv
COAD_GeneSummary.csv
LUAD_SampleSummary.csv
COAD_SampleSummary.csv
LUAD_vs_COAD_Comparison.csv
Top20_LUAD_COAD_Genes.csv
LUAD_Oncoplot.pdf
COAD_Oncoplot.pdf
LUAD_MutationSummary.pdf
COAD_MutationSummary.pdf
LUAD_vs_COAD_Top20Genes.pdf
SBS96_Plots
Activity_Plots
R Scripts
Python Scripts
Internship_Report.pdf
```

---

## Author

**Anupama Pathak**

B.Tech Biotechnology

Shri Ramswaroop Memorial University

**Internship Organization:** BioTechTrek

**Mentor:** Ms. H.S. Prithvi

**Project Duration:** 7 Weeks (Online)


