# Genome Analysis Pipeline: BLAST → Alignment → Variant Calling

## Overview
This project implements a complete, end-to-end genome resequencing pipeline applied to SARS-CoV-2: identifying unknown sequences with BLAST, aligning paired-end reads to the Wuhan reference genome with BWA, calling variants with bcftools, and characterizing mutations in Python. The three stages form a coherent workflow that mirrors real-world genomic surveillance pipelines.

## Biological Question
Given paired-end sequencing reads from an unknown sample, can we: (1) identify the organism, (2) align reads to its reference genome, and (3) characterize how the sequenced strain differs from the reference — and what those differences might mean biologically?

## Pipeline

```
Unknown sequences → [BLAST] → Identified organism + reference
                                        ↓
                              [BWA] → SAM alignment
                                        ↓
                        [SAMtools] → Quality filtered, sorted BAM
                                        ↓
                        [bcftools] → VCF variant calls
                                        ↓
                  [cyvcf2 + pandas] → Filtered variants, Ti/Tv analysis
```

## Data
- **SARS-CoV-2 paired-end reads** (FASTQ, R1 + R2): `!gdown 1J6nku7fSKE5t9yGgw0m1sUGy6regdmeb` and `!gdown 1e05YYPAHcKEjNYgwMntknvKY-Je-W2bh`
- **Wuhan-Hu-1 reference genome** (NC_045512.2): `!gdown 1yTOH-9w0RbcBhwetNWzYxFXENtiyPlzW`
- **Unknown protein sequences**: downloaded from the phagenomics GitHub repository

All data downloads automatically within the notebook.

## Methods & Tools

**Command-line tools (installed via apt/pip):**

| Tool | Stage | Purpose |
|---|---|---|
| NCBI BLAST+ (`blastp`) | Identification | Protein similarity search against NCBI nr |
| BWA (`bwa aln`, `bwa sampe`) | Alignment | Paired-end short-read alignment to reference |
| SAMtools | Processing | SAM→BAM conversion, quality filtering (MAPQ≥12), sorting, indexing |
| bcftools (`mpileup` + `call`) | Variant calling | Haploid variant calling with multi-allelic model |

**Python analysis:**

| Library | Purpose |
|---|---|
| `biopython` | BLAST XML output parsing |
| `cyvcf2` | Fast VCF file parsing |
| `pandas` | BLAST and variant result analysis |
| `matplotlib` | Variant position and quality visualization |

## Key Results
- BLAST identifies unknown sequences with high percent identity to known protein databases
- BWA alignment of SARS-CoV-2 reads to the Wuhan reference achieves high mapping rate
- Variant calling identifies mutations relative to the Wuhan-Hu-1 reference, filtered to high-confidence calls (QUAL > 200)
- Transition/transversion (Ti/Tv) ratio is computed as a variant quality metric
- C→T transitions are flagged as candidates for APOBEC3-mediated RNA editing — a known host antiviral mechanism active against SARS-CoV-2

## How to Run

**Option 1 — Google Colab (recommended)**
1. Upload `Genome_Analysis_Pipeline.ipynb` to Google Colab
2. Run all cells top to bottom — all tools install automatically and data downloads via gdown

**Option 2 — Local Linux/Mac environment**
```bash
sudo apt install bwa samtools bcftools ncbi-blast+
git clone https://github.com/yourusername/genome-analysis-pipeline
cd genome-analysis-pipeline
pip install -r requirements.txt
jupyter notebook Genome_Analysis_Pipeline.ipynb
```

## Project Structure
```
genome-analysis-pipeline/
├── README.md
├── Genome_Analysis_Pipeline.ipynb
└── requirements.txt
```
