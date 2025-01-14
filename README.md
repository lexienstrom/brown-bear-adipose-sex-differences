# Sex-Specific Gene Expression in Adipose Tissue of Hibernating Brown Bears
This repository contains code for data analysis used in our study of sex-specific gene expression in adipose tissue of brown bears during hibernation.

## Data Availability
- Raw data available at NCBI BioProject: PRJNA413091

## Contents
1. Bioinformatics Processing and Quantification
2. Differential Gene Expression Analysis
3. Gene Ontology (GO) and Kyto Encylopedia of Genes and Genomes (KEGG) Pathway Analyses

## 1. Bioinformatics Processing and Quantification
### Trimming with TrimGalore
Raw reads were quality trimmed using TrimGalore v0.4.2 with flags -q 24, --clip_R1 12, --clip_R2 12, --length 50, and --stringency 5. 

## Mapping with STAR
Trimmed reads were mapped to the brown bear reference genome assembly22 (NCBI GCA_023065955.2) using STAR v2.7.6a 23, retaining only uniquely mapping reads using – outFilterMultimapNmax 1 . 

## Quantifying gene-level read counts with featureCounts
Gene-level read counts were then quantified using featureCounts from Subread v1.6.3
