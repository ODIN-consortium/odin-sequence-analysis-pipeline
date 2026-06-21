

## Introduction

**Longread_analysis pipeline** is a bioinformatics analysis pipeline based on nfcore/taxprofiler for taxonomic classification and profiling of long-read metagenomic data. It allows for in-parallel taxonomic identification of reads or taxonomic abundance estimation with classification and profiling tool against custom-developed databases, and produces standardised output tables for facilitating results comparison between different datasets.

## Pipeline summary

![](docs/images/longread.png)

1. Input file: Concatenated long-read sequence data
2. Read QC ([`falco`](https://github.com/smithlabcode/falco))
3. Performs read pre-processing
   - Adapter clipping and merging (long-read: [porechop](https://github.com/rrwick/Porechop), [Porechop_ABI](https://github.com/bonsai-team/Porechop_ABI))
4. Low complexity and quality filtering (long-read: [Filtlong](https://github.com/rrwick/Filtlong)), [Nanoq](https://github.com/esteinig/nanoq)
5. Host-read removal (long-read: [Minimap2](https://github.com/lh3/minimap2))
   - Run merging
   - Supports statistics metagenome coverage estimation ([Nonpareil](https://nonpareil.readthedocs.io/en/latest/)) and for host-read removal ([Samtools](http://www.htslib.org/))
6. Performs taxonomic classification and/or profiling using:
   - [Kraken2](https://ccb.jhu.edu/software/kraken2/)
   - [KrakenUniq](https://github.com/fbreitwieser/krakenuniq)
   - Perform optional post-processing with: [bracken](https://ccb.jhu.edu/software/bracken/)
7. Optional assembly
8. & annotation of assembled reads ([Prokka](https://github.com/tseemann/prokka))
9. Optional annotation of antimicrobial resistance genes ([AMRFinderPlus](https://github.com/ncbi/amr))
6. Standardises output tables ([`Taxpasta`](https://taxpasta.readthedocs.io))
7. Present QC for raw reads ([`MultiQC`](http://multiqc.info/))
8. Plotting Kraken2, Bracken results ([`Krona`](https://hpc.nih.gov/apps/kronatools.html))

## Usage

> [!NOTE]
> If you are new to Nextflow and nf-core, please refer to [this page](https://nf-co.re/docs/get_started/environment_setup/overview) on how to set-up Nextflow. Make sure to [test your setup](https://nf-co.re/docs/get_started/run-your-first-pipeline) with `-profile test` before running the workflow on actual data.

First, prepare a samplesheet with your input data that looks as follows:

```csv title="samplesheet.csv"
sample,run_accession,instrument_platform,fastq_1,fastq_2,fasta
2612,barcode1,ONT,2612_barcode1.fq.gz,,
2612,barcode2,ONT,2612_barcode2.fq.gz,,
2612,barcode3,ONT,2612_barcode3.fq.gz,,
```

Each row represents a fasta (with long reads).

Additionally, you will need a database sheet that looks as follows:

```csv title="databases.csv"
tool,db_name,db_params,db_path
kraken2,db2,--quick,/<path>/<to>/kraken2/testdb-kraken2.tar.gz
```

That includes directories or `.tar.gz` archives containing databases to run the pipeline against.

Now, you can run the pipeline using:

```bash
nextflow run nf-core/taxprofiler \
   -profile <docker/singularity/.../odin> \
   --input samplesheet.csv \
   --databases databases.csv \
   --outdir <OUTDIR>  \
   --run_kraken2 --perform_longread_qc --run_krona
```

> [!WARNING]
> Please provide pipeline parameters based on individual file structure 

For more details, please refer to the [usage documentation](https://nf-co.re/taxprofiler/usage) and the [parameter 

## Pipeline output

For more details about the output files and reports, please refer to the
[output documentation](https://nf-co.re/taxprofiler/output).


## Citations


The tools used by the pipeline can be cited as follows:
> Lu J et al. Metagenome analysis using the Kraken software suite. Nature Protocols 2022 17:12 2022;17:2815–2839. https://doi.org/10.1038/s41596-022-00738-y
> 
> Chaumeil PA et al. GTDB-Tk: a toolkit to classify genomes with the Genome Taxonomy Database. Bioinformatics 2020;36:1925–1927. https://doi.org/10.1093/BIOINFORMATICS/BTZ848  

You can refer the `nf-core` publication as follows:

> **The nf-core framework for community-curated bioinformatics pipelines.**
>
> Philip Ewels, Alexander Peltzer, Sven Fillinger, Harshil Patel, Johannes Alneberg, Andreas Wilm, Maxime Ulysse Garcia, Paolo Di Tommaso & Sven Nahnsen. Nat Biotechnol._ 2020 Feb 13. doi: [10.1038/s41587-020-0439-x](https://dx.doi.org/10.1038/s41587-020-0439-x).
