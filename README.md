# FASTQ to VCF File Conversion: A Comprehensive Guide Using UseGalaxy Utility

## Objective

This repository provides a detailed guide for converting **FASTQ** files to **VCF** files using the [UseGalaxy](https://usegalaxy.org) platform. 
This is a platform designed for researchers, students, and bioinformaticians, which offers a **no-code solution** for variant calling from sequencing data, making it an easy alternative utility.

---

## Table of Contents

- [Introduction]
  - [What is a FASTQ file?]
  - [What is a VCF file?]
  - [What is UseGalaxy?]
  - [Why Use UseGalaxy?]
- [Getting Started]
- [Step-by-Step Workflow]
  - [1. Upload FASTQ Files]
  - [2. Quality Check]
  - [3. Trim Low-Quality Reads]
  - [4. Align Reads to Reference Genome]
  - [5. Convert SAM to BAM and Sort]
  - [6. Variant Calling]
- [Creating a Galaxy Workflow]
- [Conclusion]

---

## Introduction

### What is a FASTQ file?

A **FASTQ** file stores biological sequences and their quality scores from sequencing experiments. Each read includes:

- A sequence identifier
- A nucleotide sequence
- A separator
- A quality score line

![fastq_fig](https://github.com/user-attachments/assets/84eef19c-9ced-48e8-b5a6-7684d4d8e06f)

### What is a VCF file?

A **VCF (Variant Call Format)** file stores genetic variants like SNPs and indels. It includes:

- Meta-information headers
- Column headers
- Data rows (chromosome, position, reference/alternate alleles, genotype, etc.)

![vcf_format](https://github.com/user-attachments/assets/886a8d4b-db54-49c8-8cae-ee13120aecf1)


### What is UseGalaxy?

**Galaxy** is an open-source, web-based platform for bioinformatics analyses that allows users to run pipelines and workflows via a graphical interface without coding.

### Why Use UseGalaxy?

- No coding is required  
- User-friendly Graphical Interface
- Cloud-based execution
- Active community support

---

## Getting Started

### Requirements:

- `.fastq` files from your sequencing experiments/available sources such as 
- A [Galaxy account](https://usegalaxy.org)
- Stable internet connection

### Tools incorporated in the workflow

| Task               | Tools                         |
|--------------------|-------------------------------|
| Quality Check      | FastQC                        |
| Read Trimming      | Trimmomatic, TrimGalore!         |
| Read Alignment     | BWA-MEM 2          |
| MarkDuplicates     | Picard MarkDuplicates        |
| Format Conversion  | SAMtools                      |
| Variant Calling    | FreeBayes, GATK4 Mutect2 |
| Variant Filtering  | VCFtools, bcftools            |
| Variant Annotation | SnpEff, VEP, ANNOVAR          |

---

## Step-by-Step guide on how to create a Workflow

What is a workflow? A workflow is a step-by-step sequence of tools or processes that you follow to analyze biological data — often automated and repeatable. It defines what tools are used, in what order, and how data flows from one step to the next.

### 1. Upload FASTQ Files

1. Log in to [Galaxy](https://usegalaxy.org)
2. Click **Upload Data**
3. Upload via drag-and-drop or paste URL
4. Wait for the files to finish processing

Sample data can be used to run the below workflow by extracting the files from the link below:
https://docs.nvidia.com/clara/parabricks/latest/tutorials/gettingthesampledata.html#getting-the-sample-data

Quick tutorial
---

Paste the following URL in the **Paste/fetch** data tab:
https://s3.amazonaws.com/parabricks.sample/parabricks_sample.tar.gz

Once the file has been fetched, it needs to be unzipped (Unzip option present in Usegalaxy.au)

The reference FASTA file and the paired end reads (FASTQ1 and FASTQ2) are to be used for further analysis

### 2. Quality Check

- Tool to be used: **FastQC**
- Analyze:
  - Per-base quality
  - Adapter content
  - GC content
Each module provides a graphical representation of the data, accompanied by a color-coded flag indicating the quality:
Green (PASS): No significant issues detected.
Yellow (WARN): Potential issues that may require attention.
Red (FAIL): Serious problems that need to be addressed before proceeding 

### 3. Trim Low-Quality Reads (Optional)

- Tool to be used: **Trimmomatic; TrimGalore!**
- Remove adapters and low-quality bases
Trimmomatic operates by applying a series of user-defined steps to each read or read pair in a specified order. This pipeline-based architecture allows for efficient and customizable preprocessing of sequencing data. It supports both single-end and paired-end data formats and can handle compressed files

Trim Galore! is a bioinformatics tool designed to automate the process of quality and adapter trimming for high-throughput sequencing data, particularly focusing on bisulfite sequencing applications like Reduced Representation Bisulfite Sequencing (RRBS).

### 4. Align Reads to Reference Genome

- Tools: **BWA-MEM** or **Bowtie2**
- Input: Clean FASTQ + reference genome
- Output: `.sam` file
- Aligns reads to a reference genome (e.g., hg38 which is an in built reference or reference fasta provided)
BWA-MEM2 is an advanced read alignment tool designed to efficiently map high-throughput sequencing reads to a reference genome. It is the successor to the widely used BWA-MEM algorithm and offers significant performance improvements.
The reference genome used can either built in or a specific reference FASTA can be provided.

### 5. Convert SAM to BAM and Sort

- Use **SAMtools** to:
  - Sort and indexing
SortSam organizes the records in a SAM or BAM file based on a specified sort order

### 6. Mark Duplicates - Picard

- Use **Picard MarkDuplicates** to:
- Identify and flag PCR duplicates to avoid false variants
    Input: BAM
    Output: Deduplicated BAM
The tool scans aligned reads to identify duplicates—reads that originate from the same DNA fragment. This process helps in distinguishing true biological variants from artifacts introduced during sample preparation.

### 6. Variant Calling

- Tools: **FreeBayes** or **GATK4 Mutect2**
- Input: Sorted BAM + reference
- Output: Raw `.vcf` file

FreeBayes is an open-source, haplotype-based variant caller designed to detect small genetic polymorphisms—such as single-nucleotide polymorphisms (SNPs), insertions and deletions (indels), multi-nucleotide polymorphisms (MNPs), and complex events—by analyzing short-read sequencing data aligned to a reference genome.

GATK4 Mutect2 is a specialized tool for detecting somatic mutations—such as single nucleotide variants (SNVs) and small insertions and deletions (indels)—in tumor DNA samples. It employs a haplotype-based approach to accurately identify variants, even in complex regions of the genome.

---

## Creating a Galaxy Workflow

1. Go to **Workflow > Create New Workflow**
2. Name your workflow (e.g., `FASTQ_to_VCF_Workflow`)
3. Add tools:
   - FastQC → Trimmomatic → BWA → SAMtools → GATK4 Mutect2
4. Link input/output ports
5. Save and **Run** with your data
6. Analyse the outputs
7. **Share or Publish** your workflow

**Table 2** : Summary table of Tools used in the workflow and the descripton

| Step | Tool Name | Description |
|------|-----------|-------------|
| 1 | **FastQC** | Performs quality checks on raw FASTQ files |
| 2 | **Trim Galore** | Automatically trims Illumina adapters and low-quality bases using Cutadapt; includes optional FastQC reporting |
| 3 | **BWA-MEM2** | Aligns reads to a reference genome (e.g., hg38 which is an in built reference or reference fasta provided) |
| 4 | **Picard MarkDuplicates** | Identifies and marks duplicate reads |
| 5 | **SortSam** | Sorts SAM/BAM files by coordinate or name using Picard; prepares files for downstream analysis |
| 6 | **GATK4 Mutect2** | Calls somatic variants in tumor or tumor-normal pairs using a Bayesian model; optimized for cancer mutation detection |

The below workflow can be used as a reference to build your very own: https://usegalaxy.org/u/varshitha/w/fastq-2-vcf-workflow 
##DISCLAIMER: The provided workflow is just a reference and is not to be used for directly performing FASTQ-> VCF conversion
 
## Conclusion

With UseGalaxy, you can build powerful, **no-code pipelines** for processing sequencing data into annotated genetic variants. This guide walks you through a beginner-friendly process from **FASTQ to VCF** using trusted tools and reproducible workflows.
