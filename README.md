# QIIME2 Metagenomics Pipeline

## Overview
This repository contains an end-to-end microbiome analysis workflow using QIIME2 for taxonomic profiling, diversity analysis, and visualization of 16S rRNA sequencing data.

The pipeline demonstrates practical bioinformatics skills including data preprocessing, denoising, taxonomic classification, diversity analysis, and reproducible workflow management.

---

# Project Objectives

- Perform microbiome taxonomic analysis
- Process raw FASTQ sequencing data
- Generate high-quality ASVs using DADA2
- Perform alpha and beta diversity analysis
- Visualize microbial community composition
- Build a reproducible bioinformatics workflow

---

# Dataset

- Sample Type: Oral microbiome samples
- Sequencing Platform: Illumina
- Data Type: 16S rRNA sequencing
- Groups:
  - Healthy
  - OPMD
  - OSCC

---

# Tools & Technologies

| Tool | Purpose |
|------|----------|
| QIIME2 | Microbiome analysis |
| DADA2 | Denoising and taxonomy-feature table generation
| Python | Data handling and automation |
| Linux | Command-line workflow |
| Git/GitHub | Version control |

---

**Workflow**
 1. Import FASTQ Files

```bash
qiime tools import \
--type 'SampleData[SequencesWithQuality]' \
--input-path manifest.csv \
--output-path demux.qza \
--input-format SingleEndFastqManifestPhred33V2

2. Quality Visualization
qiime demux summarize \
--i-data demux.qza \
--o-visualization demux.qzv

3.Denoising using DADA2
qiime dada2 denoise-single \
--i-demultiplexed-seqs demux.qza \
--p-trim-left 20 \
--p-trunc-len 220 \
--o-table table.qza \
--o-representative-sequences rep-seqs.qza \
--o-denoising-stats stats.qza

4.Taxonomic Classification

qiime feature-classifier classify-sklearn \
--i-classifier classifier.qza \
--i-reads rep-seqs.qza \
--o-classification taxonomy.qza

5.Taxonomy Visualization
qiime taxa barplot \
--i-table table.qza \
--i-taxonomy taxonomy.qza \
--m-metadata-file metadata.tsv \
--o-visualization taxa-barplot.qzv

6.Diversity Analysis

qiime diversity core-metrics \
--i-table table.qza \
--p-sampling-depth 10000 \
--m-metadata-file metadata.tsv \
--output-dir core-metrics-results

******Paired-end*********

---

# Paired-End Analysis Workflow

## Import Paired-End FASTQ Files

```bash
qiime tools import \
--type 'SampleData[PairedEndSequencesWithQuality]' \
--input-path manifest.csv \
--output-path paired-end-demux.qza \
--input-format PairedEndFastqManifestPhred33V2
```

---

## Paired-End Quality Visualization

```bash
qiime demux summarize \
--i-data paired-end-demux.qza \
--o-visualization paired-end-demux.qzv
```

---

## Paired-End Denoising using DADA2

```bash
qiime dada2 denoise-paired \
--i-demultiplexed-seqs paired-end-demux.qza \
--p-trim-left-f 20 \
--p-trim-left-r 20 \
--p-trunc-len-f 240 \
--p-trunc-len-r 200 \
--o-table paired-table.qza \
--o-representative-sequences paired-rep-seqs.qza \
--o-denoising-stats paired-stats.qza
```

---

## Paired-End Taxonomic Classification

```bash
qiime feature-classifier classify-sklearn \
--i-classifier classifier.qza \
--i-reads paired-rep-seqs.qza \
--o-classification paired-taxonomy.qza
```

---

## Paired-End Taxonomy Visualization

```bash
qiime taxa barplot \
--i-table paired-table.qza \
--i-taxonomy paired-taxonomy.qza \
--m-metadata-file metadata.tsv \
--o-visualization paired-taxa-barplot.qzv
