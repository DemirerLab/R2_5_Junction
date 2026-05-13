# R2 5′ Junction Classification

The workflow outlined here classifies the 5′ junctions formed between host 25S rDNA and the R2Tg transgene insert in *Nicotiana benthamiana* using ONT amplicon long-read sequencing. Reads spanning the complete junction are first filtered from raw FASTQ files using Cutadapt, then each junction is classified as anneal, join, or snap-back following the framework of McIntyre *et al.*, 2026. The detected R2 5′ UTR form — R33 (full-length) or R28 (5′-cleaved) — is recorded alongside the junction category for each read.

## Set up

This workflow has been developed and tested on MacOS Apple Silicon. We provide one environment file, *r2_junctionseq.yaml*, covering all pipeline steps.

### r2_junctionseq Environment Install

```bash
conda env create -f r2_junctionseq.yaml   # creates r2_junctionseq
```

## Step 1: File organization

The workflow requires one FASTQ file per replicate from ONT amplicon sequencing, along with the transgene insert sequence and the 25S rDNA reference sequence. The insert and rDNA reference files are provided in this repository as *insert.fasta* and *rDNA.fasta*, respectively. Our four replicate *N. benthamiana* FASTQ files are available at [SRA](https://www.ncbi.nlm.nih.gov/sra) under accession **PRJNA1463879**. To be compatible with the workflow your directory should look like this:

```
- R2-5J-classification-main/ (name of this folder can be changed)
│   ├── 01_cutadapt_filter.sh
│   ├── 02_classify_junctions.ipynb
│   ├── r2_junctionseq.yaml
│   ├── insert.fasta
│   ├── rDNA.fasta
│   ├── raw_data/
│   │   └── 1.fastq
│   │   └── 2.fastq
│   │   └── 3.fastq
│   │   └── 4.fastq
│   ├── cutadapt_filtered/
│   ├── classified/
```

## Step 2: Cutadapt filtering

Activate the environment and run the cutadapt filter script to retain only reads that span the complete 5′ junction. The script processes each FASTQ file in `raw_data/` through two sequential cutadapt passes and writes filtered reads to `cutadapt_filtered/`.

```bash
conda activate r2_junctionseq
bash 01_cutadapt_filter.sh
```

## Step 3: Run the classification pipeline

Open `02_classify_junctions.ipynb` and follow the in-built instructions. Junction classification, per-sample summary statistics, R33/R28 breakdown, and all figures matching those in our publication will be generated and saved to the `classified/` directory.

## References

- McIntyre *et al.* (2026) *Science* 391, 885. Different DNA repair pathways support intact or truncated insertions by R2 retrotransposon protein. [doi:10.1126/science.adz3121](https://doi.org/10.1126/science.adz3121)
