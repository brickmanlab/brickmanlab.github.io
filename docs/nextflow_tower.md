# Nextflow Tower

This is a guide on how to use [Nextflow Tower](https://help.tower.nf/23.1/) to monitor nf-core pipeline runs on DanGPU.

All [nf-core pipelines](https://nf-co.re/pipelines) have been successfully configured for use on DanGPU.


## **Getting started**
If this is the first time you use Nextflow Tower, [Sign in](https://tower.nf/login) and create a [personal access token](https://tower.nf/tokens)

You need to create a samplesheet before running any nf-core pipeline. Samplesheet format varies according to pipeline and examples can be found in the usage docs:

nf-core/rnaseq [usage docs](https://nf-co.re/rnaseq/3.11.2/usage)

nf-core/chipseq [usage docs](https://nf-co.re/chipseq/2.0.0/usage)

nf-core/cutandrun [usage docs](https://nf-co.re/cutandrun/3.1/usage)



## **Running pipelines**

Use the helper script **nf-core_tower.sh** to run DanGPU nf-core configs with Tower. 

- cd .local/bin

- Start a new tmux session:

    tmux new -s session_name

- Export your personal tower access token:

    export TOWER_ACCESS_TOKEN=your_access_token

- Launch desired nf-core pipeline using helper script. Usage is sh nf-core_tower.sh RUNNAME nextflow run OPTIONS. As a minimum, the pipeline name, samplesheet location, and genome must be defined, e.g. for rnaseq:

    sh nf-core_tower.sh MYPAPER_2023 nextflow run nf-core/rnaseq -r 3.8.1 --input samplesheet.csv --genome mm10




## **Tower CLI installation**

The Tower CLI excecutable is **already installed** on DanGPU at ./local/bin/

- Download the latest version of Tower CLI:

    wget https://github.com/seqeralabs/tower-cli/releases/download/v0.7.3/tw-0.7.3-linux-x86_64

- Make the file executable and move to directory accessible by $PATH variable:

    mv tw-0.7.3-linux-x86_64 tw-agent

    chmod +x tw

    mkdir .local/bin

    mv tw ./local/bin/ 


## **References**
[Tower CLI configuration](https://github.com/seqeralabs/tower-cli/#2-configuration)

[Tower Agent](https://help.tower.nf/22.3/agent/)