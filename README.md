
# Introduction
WGS_analysis pipeline is a bioinformatics pipeline based on Bactopia for complete analysis of bacterial genomes using multiple tools. 



## Pipeline summary
![Pipeline Overview](data/wgs.png)


This pipeline is a bioinformatics workflow for whole-genome sequencing (WGS) analysis. The WGS_analysis pipeline is built with [Nextflow](https://www.nextflow.io/) It takes raw sequencing reads as input and performs read preprocessing, quality control, genome assembly, taxonomic assignment, functional annotation, antimicrobial resistance detection, MLST typing, cg/wgMLST analysis, and phylogenetic tree construction.

The workflow is designed to provide a reproducible end-to-end analysis starting from raw FASTQ files and producing standardised outputs for downstream genomic epidemiology, pathogen surveillance, and comparative genomics.

The pipeline performs the following steps:

1. **Input data**
   - Accepts raw sequencing reads, typically paired-end FASTQ files.

2. **Adapter removal and read trimming**
   - Removes sequencing adapters and low-quality bases using [Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic).

3. **Read quality control**
   - Performs quality assessment of raw and/or trimmed reads using [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/).
   - Samples that do not pass defined quality-control thresholds can be flagged or aborted before downstream analysis.

4. **Genome assembly**
   - Assembles quality-filtered reads into draft genome assemblies using [SPAdes](https://github.com/ablab/spades).
   - Assembly quality thresholds can be used to stop low-quality samples before further analysis.

5. **Taxonomy mapping**
   - Assigns taxonomy to assembled genomes using [GTDB-Tk](https://github.com/Ecogenomics/GTDBTk), based on the Genome Taxonomy Database framework.

6. **Functional annotation**
   - Annotates assembled genomes and predicts coding sequences, rRNAs, tRNAs, and other genomic features using [Prokka](https://github.com/tseemann/prokka).

7. **Antimicrobial resistance detection**
   - Screens assembled genomes for antimicrobial resistance genes and resistance-associated point mutations using [AMRFinderPlus](https://github.com/ncbi/amr).

8. **MLST typing**
   - Performs multi-locus sequence typing using [stringMLST](https://github.com/jordanlab/stringMLST).
   - MLST allele and sequence type information is retrieved using the [PubMLST](https://pubmlst.org/) database.

9. **cg/wgMLST analysis**
   - Performs core genome or whole genome MLST allele calling using [chewBBACA](https://github.com/B-UMMI/chewBBACA).

10. **Phylogenetic tree construction**
    - Builds a phylogenetic tree from allelic profiles or genome comparison outputs using [GrapeTree](https://github.com/achtman-lab/GrapeTree).


## Main outputs

The pipeline generates the following major output files and reports:

- Trimmed FASTQ files from adapter and quality trimming
- FastQC quality-control reports
- Draft genome assemblies in FASTA format
- Taxonomic classification results from GTDB-Tk
- Functional genome annotation files from Prokka
- Antimicrobial resistance gene and mutation reports from AMRFinderPlus
- MLST sequence type results
- cg/wgMLST allele profiles from chewBBACA
- Phylogenetic tree files and visualisation-ready outputs from GrapeTree
- Log files and summary reports for tracking pipeline execution and sample status

## Intended use

This workflow is intended for genomic analysis of whole-genome sequencing data. It can be used for pathogen surveillance, outbreak investigation, comparative genomics, antimicrobial resistance monitoring, and generation of standardised genomic outputs for downstream interpretation.


# Usage
```
mamba create -y -n bactopia -c conda-forge -c bioconda bactopia
conda activate bactopia
bactopia datasets

# Paired-end
bactopia --R1 R1.fastq.gz --R2 R2.fastq.gz --sample SAMPLE_NAME \
         --datasets datasets/ --outdir OUTDIR

# Single-End
bactopia --SE SAMPLE.fastq.gz --sample SAMPLE --datasets datasets/ --outdir OUTDIR

# Multiple Samples
bactopia prepare MY-FASTQS/ > fastqs.txt
bactopia --fastqs fastqs.txt --datasets datasets --outdir OUTDIR

# Single ENA/SRA Experiment
bactopia --accession SRX000000 --datasets datasets --outdir OUTDIR

# Multiple ENA/SRA Experiments
bactopia search "staphylococcus aureus" > accessions.txt
bactopia --accessions accessions.txt --dataset datasets --outdir ${OUTDIR}
```


# Citation
Petit III RA, Read TD, *Bactopia: a flexible pipeline for complete analysis of bacterial genomes.* __mSystems__. 5 (2020), https://doi.org/10.1128/mSystems.00190-20.




