# FASTQ to VCF File Conversion: A Comprehensive Guide Using UseGalaxy Utility

## 🧬 Objective

This repository provides a detailed guide for converting **FASTQ** files to **VCF** files using the [UseGalaxy](https://usegalaxy.org) platform. 
This is a platform designed for researchers, students, and bioinformaticians, which offers a **no-code solution** for variant calling from sequencing data, making it an easy alternative utility.

---

## 📚 Table of Contents

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

## 📘 Introduction

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

- 🔧 No coding is required  
- 🖱️ User-friendly Graphical Interface
- ☁️ Cloud-based execution
- 🌍 Active community support

---

## 🚀 Getting Started

### Prerequisites

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

## 🔄 Step-by-Step guide on how to create a Workflow

What is a workflow? A workflow is a step-by-step sequence of tools or processes that you follow to analyze biological data — often automated and repeatable. It defines what tools are used, in what order, and how data flows from one step to the next.

### 1. Upload FASTQ Files

1. Log in to [Galaxy](https://usegalaxy.org)
2. Click **Upload Data**
3. Upload via drag-and-drop or paste URL
4. Wait for the files to finish processing

Sample data can be used to run the below workflow using the steps mentioned in the link provided below:
https://docs.nvidia.com/clara/parabricks/latest/tutorials/gettingthesampledata.html#getting-the-sample-data


### 2. Quality Check

- Use **FastQC**
- Analyze:
  - Per-base quality
  - Adapter content
  - GC content

### 3. Trim Low-Quality Reads (Optional)

- Tools: **Trimmomatic** or **Cutadapt**
- Remove adapters and low-quality bases

### 4. Align Reads to Reference Genome

- Tools: **BWA-MEM** or **Bowtie2**
- Input: Clean FASTQ + reference genome
- Output: `.sam` file
- Aligns reads to a reference genome (e.g., hg38 which is an in built reference or reference fasta provided)

### 5. Convert SAM to BAM and Sort

- Use **SAMtools** to:
  - Convert SAM to BAM
  - Sort BAM
  - (Optional) Index BAM

### 6. Mark Duplicates - Picard

- Use **Picard MarkDuplicates** to:
- Identify and flag PCR duplicates to avoid false variants
    Input: BAM
    Output: Deduplicated BAM
    Purpose: .

### 6. Variant Calling

- Tools: **FreeBayes** or **GATK HaplotypeCaller**
- Input: Sorted BAM + reference
- Output: Raw `.vcf` file

### 7. Filter Variants

- Tools: **VCFtools** or **bcftools**
- Apply filters such as:
  - Minimum `QUAL`
  - Minimum read `DP`

### 8. (Optional) Annotate Variants

- Tools: **SnpEff**, **VEP**, or **ANNOVAR**
- Output: Annotated `.vcf` or summary table

**Table 2** : Summary table of Tools used in the workflow and the descripton

| Step | Tool Name | Description |
|------|-----------|-------------|
| 1 | **FastQC** | Performs quality checks on raw FASTQ files |
| 2 | **Trim Galore** | Automatically trims Illumina adapters and low-quality bases using Cutadapt; includes optional FastQC reporting |
| 3 | **BWA-MEM2** | Aligns reads to a reference genome (e.g., hg38 which is an in built reference or reference fasta provided) |
| 4 | **Picard MarkDuplicates** | Identifies and marks duplicate reads |
| 5 | **SortSam** | Sorts SAM/BAM files by coordinate or name using Picard; prepares files for downstream analysis |
| 6 | **GATK4 Mutect2** | Calls somatic variants in tumor or tumor-normal pairs using a Bayesian model; optimized for cancer mutation detection |

---

## 🧩 Creating a Galaxy Workflow

1. Go to **Workflow > Create New Workflow**
2. Name your workflow (e.g., `FASTQ_to_VCF_Workflow`)
3. Add tools:
   - FastQC → Trimmomatic → BWA → SAMtools → FreeBayes → VCFtools
4. Link input/output ports
5. Save and **Run** with your data
6. (Optional) **Share or Publish** your workflow

---

## ✅ Conclusion

With UseGalaxy, you can build powerful, **no-code pipelines** for processing sequencing data into annotated genetic variants. This guide walks you through a beginner-friendly process from **FASTQ to VCF** using trusted tools and reproducible workflows.

Need help? Visit the [Galaxy Help Forum](https://help.galaxyproject.org/)
