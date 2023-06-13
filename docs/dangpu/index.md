# DanGPU

For starting on the server make sure to read:

- [DanGPU manual](https://sgn102.pages.ku.dk/a-not-long-tour-of-dangpu/)
- [Genomics Platform wiki](https://sundgenomics.github.io)
- **platforms**: [JupyterHub](http://dangpu01fl:8989) and [RStudio](http://dangpu01fl:8787)

## First time on server

### 1. Run the command to do basic setup and logout afterwards

```bash
/maps/projects/dan1/apps/etc/init_dangpu_env.sh && \
echo "export MODULEPATH=/maps/projects/dan1/apps/.modules:\${MODULEPATH}" >> $HOME/.bash_profile
```

### 2. Create extra simlinks for folders

```bash
ln -s /maps/projects/dan1/people/$USER $HOME/datadir
ln -s /projects/dan1/data/Brickman $HOME/
```

### 3. Setup `.bash_profile`

To make your life simple, run the command below. This will give you access to
extra helper scripts we have developed.

```bash
echo "export PATH=/projects/dan1/data/Brickman/helper-scripts/helper-scripts:${PATH}" >> ~/.bash_profile
```

## Folder structure
