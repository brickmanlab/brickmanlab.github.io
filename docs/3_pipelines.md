# Running pipelines

By default, we run [nf-core](https://nf-co.re) pipelines. To run a pipeline, read
the official [documentation with an example](https://nf-co.re/configs/ku_sund_danhead).

## Basic workflow for running an nf-core pipeline

You will need at least 3 things to run a pipeline:

- A slurm (sbatch) script that will run the pipeline on one the computing nodes
- A parameter file which contains the options specific for your experiment
- A samplesheet, typically specific for the pipeline in question. Sometimes (such as is the case with the PRIME-seq pipeline), you will need a few more csv files

### The samplesheet

You will need to follow the instructions specific to the experiment you have done and pipeline you wish to run.

An example of an RNA-seq samplesheet is taken from the [nf-core website](https://nf-co.re/rnaseq/3.14.0/):

``` bash
sample,fastq_1,fastq_2,strandedness
CONTROL_REP1,AEG588A1_S1_L002_R1_001.fastq.gz,AEG588A1_S1_L002_R2_001.fastq.gz,auto
CONTROL_REP1,AEG588A1_S1_L003_R1_001.fastq.gz,AEG588A1_S1_L003_R2_001.fastq.gz,auto
CONTROL_REP1,AEG588A1_S1_L004_R1_001.fastq.gz,AEG588A1_S1_L004_R2_001.fastq.gz,auto
```
As good practice, place the full path of the FASTQ files, e.g.: /maps/projects/dan1/data/Brickman/assays/RNA_ABC_19700101/raw/SAMPLE_S1_L001_R1_001.fastq.gz

It might be easier to make this sheet in Excel and then export it as a csv file.

Save or copy this file as samplesheet.csv in your assay folder.

### Parameters

A params.yml file contains the options you can customise for your run or experiment.

``` bash
input:  /maps/projects/dan1/data/Brickman/assays/RNA_ABC_19700101/samplesheet.csv
outdir: /projects/dan1/scratch/temp/abc123/19700101_ABC_RNAseq
fasta: /maps/projects/dan1/data/Brickman/shared/references/homo_sapiens/ensembl/GRCh38_110/Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa
aligner:  star_rsem
gtf:  /maps/projects/dan1/data/Brickman/shared/references/homo_sapiens/ensembl/GRCh38_110/Homo_sapiens.GRCh38.110.gtf
skip_pseudo_alignment:  true
```

Here I have specified the genome and gene annotation files I would like to use. This will depend on your experiment.

### A slurm script

Here is an example of a slurm script required. Modify it to your liking.

``` bash
#!/bin/bash

#SBATCH --job-name=wooo_pipelines    # job name
#SBATCH --mail-type=NONE    # mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH -c 1    # number of requested cores
#SBATCH --mem=4gb    # total requested RAM
#SBATCH --time=2-00:00:00               # max. running time of the job, format in D-HH:MM:SS
#SBATCH --output=pipeline_%j.log   # Standard output and error log, '%j' gives the job ID
#SBATCH -w dancmpn02fl # dancmpn01fl or dancmpn02fl

ml purge
ml openjdk/20.0.0 nextflow/23.04.1.5866 singularity/3.8.0

nextflow run nf-core/rnaseq \
    -r 3.14.0 \
    -with-tower \
    -profile ku_sund_danhead,dancmpn02fl # The second bit needs to match the SBATCH -w at the top; send the run to the same server!

```

To schedule the script:

``` bash
sbatch pipeline.sbatch
```


## Monitoring runs with Nextflow Tower

This is a guide on how to use [Nextflow Tower](https://help.tower.nf/23.1/) to
monitor nf-core pipeline runs.

We have created an API token for our GitHub account (brickmanlab) and restricted
it to run only pipelines, nothing else. The `TOWER_WORKSPACE_ID` and `TOWER_ACCESS_TOKEN`
are stored in `Brickman/config/brickman.bashrc`.

To do more advance stuff, you have to create your own [personal access token](https://tower.nf/tokens).

### Tower CLI installation

The tower cli[^1] is required to be installed only once to connect the server as
a computing resource. Afterward, it's not required any more[^2].

``` bash
# Download the latest version of Tower CLI:
wget https://github.com/seqeralabs/tower-cli/releases/download/v0.7.3/tw-0.7.3-linux-x86_64

# Make the file executable and move to directory accessible by $PATH variable:
mkdir ~/.local/bin && mv tw-* tw && chmod +x ~/.local/bin/tw
```
## Failed to pull singularity image error

If you see the following error in your nextflow.log or in tower:

```
Failed to pull singularity image
```

Then you need to force download the images you need to run the pipeline. Singularity images are specific to __both__ the computing server and the version of the pipeline.

Create an interactive session in the server you would like to send your pipeline to.

``` bash
srun -c 2 --mem=8gb -w [dancmpn01fl OR dancmpn02fl] --time=0-04:00:00 -J mBanana --pty bash
```

Load in a few modules

``` bash
ml dangpu_libs python/3.8.16 nextflow nf-core/2.13.1a singularity
```

Download the containers for a particular pipeline

``` bash
nf-core download --revision [PIPELINE_VERSION] --compress none --container-system singularity [PIPELINE]

```

An example for version 3.14.0 of the nf-core RNA-seq  pipeline is:

``` bash
nf-core download --revision 3.14.0 --compress none container-system singularity nf-core/rnaseq
```


[^1]: [Tower CLI configuration](https://github.com/seqeralabs/tower-cli/#2-configuration)
[^2]: [Tower Agent](https://help.tower.nf/22.3/agent/)
