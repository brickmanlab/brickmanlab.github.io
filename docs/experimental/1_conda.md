# Building environments with conda, uv and mamba

This experimental workflow uses `conda` to create environments and `uv` and `mamba` to install and solve environment packages

## Conda

We have shared `conda` environments to simplify your life.

- To list all available envs: `conda env list`
- To activate env: `source activate brickman`

### Creating own shared environment

To create a shared environment and make it writable by everyone:

```bash
module load miniconda/latest uv/latest

conda create --prefix /maps/projects/dan1/data/Brickman/conda/envs/${NAME_OF_YOUR_ENV} python=${VERSION_OF_PYTHON}
source activate ${NAME_OF_YOUR_ENV}

chmod -R 755 /maps/projects/dan1/data/Brickman/conda/envs/brickman/${NAME_OF_YOUR_ENV}
```

### Install packages from PyPi with uv

[Documentation for uv](https://docs.astral.sh/uv/pip/packages/)

While in the created environment, install packages with the command

```bash
uv pip install [packages]
```

### Install packages from bioconda using mamba

For packages in bioconda, use `mamba`

```bash
mamba install [packages]
``` 

### Create an environment.yml from your conda environment

When you are happy with the environment, create an environment.yml to make it portable

```bash
conda export --file={NAME_OF_ENVIRONMENT}.brickmanCondaEnv.yaml
```

