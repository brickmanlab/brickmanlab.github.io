# Conda & modules

## Conda

If you work with `conda` you can use `mamba` instead, which is **faster** tool
to install packages.

We created shared `conda` environments to simplify your life.

- To list all available envs: `conda env list`
- To activate env: `source activate brickman`

### Creating own shared environment

Here is an example how we created shared environment called `brickman`.

```bash
module load miniconda/latest

conda create --prefix /maps/projects/dan1/data/Brickman/conda/envs/brickman python=3.10
source activate brickman
pip install cruft cookiecutter

chmod -R 755 /maps/projects/dan1/data/Brickman/conda/envs/brickman
```

To install shared `conda` environment for the lab, follow the steps below.

1. Create a configuration file which contains the list of packages you want
  to use. Use prefix `brickman-<NGS>.yml`
2. Populate the configuration file
3. Create the conda environment

```bash
mamba env create -p /projects/dan1/data/Brickman/conda/envs/brickman-<NGS>.yml -f brickman-<NGS>.yml
```

### Example conda environment

Configuration for `brickman-chipseq` environment.

```yaml
name: brickman-chipseq
channels:
  - conda-forge
  - bioconda
  - anaconda
  - defaults
dependencies:
  - bioconda::bedtools==2.31.0
  - bioconda::deeptools==2.31.0
  - bioconda::homer==4.11
  - bioconda::intervene==0.6.4
  - bioconda::macs2==2.2.9.1
  - bioconda::pygenometracks==3.8
  - bioconda::seacr==1.3
  - bioconda::samtools==1.17
prefix: /projects/dan1/data/Brickman/conda/envs/brickman-chipseq
```

To install the environment, run

```bash
mamba env create -p /projects/dan1/data/Brickman/conda/envs/brickman-chipseq -f brickman-chipseq.yml
```

## Modules

```bash
module avail

module load miniconda/latest
```
