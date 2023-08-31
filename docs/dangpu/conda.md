# Conda

To install shared `conda` environment for the lab, follow the steps below.

1. Create a configuration file which contains the list of packages you want
  to use. Use prefix `brickman-<NGS>.yml`
2. Populate the configuration file
3. Create the conda environment

```bash
mamba env create -p /projects/dan1/data/Brickman/conda/envs/brickman-<NGS>.yml -f brickman-<NGS>.yml
```

## Example case

Configuration for `brickman-chipseq` environment.

``` title="brickman-chipseq.yml"
--8<-- "./docs/conda/brickman-chipseq.yml"
```

To install the environment, run

```bash
mamba env create -p /projects/dan1/data/Brickman/conda/envs/brickman-chipseq -f brickman-chipseq.yml
```
