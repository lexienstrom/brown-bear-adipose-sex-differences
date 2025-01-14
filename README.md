# Sex-Specific Gene Expression in Adipose Tissue of Hibernating Brown Bears
This repository contains code for data analysis used in our study of sex-specific gene expression in adipose tissue of brown bears during hibernation.

## Data Availability
- Raw data available at NCBI BioProject: PRJNA413091
- Brown bear genome assembly: [GCF_023065955.2](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_023065955.2/)

## Contents
1. Bioinformatics Processing and Quantification
2. Differential Gene Expression Analysis
3. Gene Ontology (GO) and Kyoto Encylopedia of Genes and Genomes (KEGG) Pathway Analyses

## 1. Bioinformatics Processing and Quantification

Note: These analyses were run on the Washington State University HPC (Kamiak). For generalizability, simplified commands are presented here rather than the specific SLURM scripts used to run these commands on Kamiak.

### Trimming with TrimGalore
Raw reads were quality trimmed using TrimGalore v0.4.2 with flags -q 24, --clip_R1 12, --clip_R2 12, --length 50, and --stringency 5. 
```bash
trim_galore --paired -q 24 --fastqc --fastqc_args "--noextract --nogroup --outdir 1_trim/fastqc" --stringency 5 --illumina --length 50
-o 1_trim --clip_R1 12 --clip_R2 12 [path/to/read1] [path/to/read2]
```

### Mapping with STAR
Trimmed reads were mapped to the brown bear reference genome assembly22 (NCBI GCA_023065955.2) using STAR v2.7.6a 23, retaining only uniquely mapping reads using – outFilterMultimapNmax 1.

```bash
STAR --genomeDir ../2_mapping/reference/GCF_023065955.2 --runThreadN 12 --readFilesIn [path/to/read1] [path/to/read2] --outFileNamePrefix ./2_mapped_test/[identifier prefix]  --outFilterMultimapNmax 1  --readFilesCommand zcat  --twopassMode Basic

# Convert resulting sam file to sorted bam file
samtools sort [path/to/star/output/sam]  -o [path/to/sorted/bam]

```

### Quantifying gene-level read counts with featureCounts
Gene-level read counts were then quantified using featureCounts from Subread v1.6.3
```bash
featureCounts -p -F 'GTF' -T 8 -t exon -g gene_id -a [path/to/GTF] -o [outfile.txt] [path/to/sorted/bam/files/*.bam]
```

## 2. Differential Gene Expression Analysis
The following R script contains code to
- Normalize gene expression counts for adipose
- Perform differential expression analysis between active season females and hibernation females, and active season males and hibernating males and perform differential expression analysis between active  males and active females, and hibernating  males and hibernating females
- Plot PCA 
- Subset significantly differentially expressed genes
- Plot Venn diagram of DEGs
- Subset unique upregulated and downregulated genes for males and females
- Subset shared upregulated and downregulated genes, plot the male vs female log2fc values of shared genes
- Make heatmaps of DEGs
- Plot the transcripts per million of two interesting genes (GPER1 and OXTR)

Link to R Markdown: [adiposeGenes_DESeq2.11.24.24.Rmd](https://github.com/lexienstrom/brown-bear-adipose-sex-differences/blob/main/analysis/adipose_DESeq2.11.25.24.Rmd)

## 3. GO and KEGG Pathway Analysis of Differentially Expressed Genes
The following R script contains code used to
- Use Gene Set Enrichment Analysis (GSEA)to characterize enriched GO and KEGG terms for all male and female hibernation vs active season genes, and all active and hibernation male vs female genes
  * Plot GSEA results as dotplot
  * Save GSEA results as csv files
- Use Overrepresentation Analysis (ORA) to characterize enriched GO and KEGG terms for differentially expressed male-specific and female-specific up and downregulated genes
  * Plot ORA results as bargraphs
  * Save ORA results as csv files

Link to R Markdown: [go_and_KEGG_analysis.11.30.2024.Rmd](https://github.com/lexienstrom/brown-bear-adipose-sex-differences/blob/main/analysis/go_and_KEGG_analysis.11.30.2024.Rmd)
