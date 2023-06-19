# DanGPU

For starting on the server make sure to read:

- [DanGPU manual](https://sgn102.pages.ku.dk/a-not-long-tour-of-dangpu/)
- [Genomics Platform wiki](https://sundgenomics.github.io)
- **platforms**: [JupyterHub](http://dangpu01fl:8989) and [RStudio](http://dangpu01fl:8787)

## First time on server

We wrote a small script which will setup everything for you. Login to the server
and run the following command.

```bash
sh /maps/projects/dan1/data/Brickman/helper-scripts/brickman-setup.sh
```

## Working with modules

```bash
module load miniconda/latest
```

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
