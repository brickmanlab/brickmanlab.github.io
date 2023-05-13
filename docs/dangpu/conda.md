# Conda

## Setup on DanGPU

```yaml
channels:
  - conda-forge
  - bioconda
  - defaults
envs_dirs:
  - /projects/dan1/people/$USER/envs/
  - ~/projects/data/Brickman/conda/envs
pkgs_dirs:
  - ~/.conda/pkgs/
```

## Setup on KU-computer

Go [here](https://docs.conda.io/en/latest/miniconda.html) and download
Miniconda PKG not BASH. If you're running **M1/2** please follow this
[guideline](https://towardsdatascience.com/how-to-manage-conda-environments-on-an-apple-silicon-m1-mac-1e29cb3bad12).

### Example for CHIP-seq setup

```bash
conda create --name chipseq python=3.6
conda activate chipseq
conda install -c bioconda deeptools bedtools
pip install intervene
```
