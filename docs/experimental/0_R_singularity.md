# Running R Singularity images in VSCode

Given the friction of maintaining R instances and libraries in the server, we
have begun testing the use of all-in-one singularity images for standard R
workflows.
This has the advantage of using ipynb files for everything and not needing to
deal with the jankiness of R in VSCode. In addition, it guarantees the versions
of the libraries installed play nice.

There are however a few downsides:
	- The images are immutable. You have access to the libraries within the image and cannot add libraries from your personal R library
	- The Brickman lab `assays/` and `projects/` shortcuts do not work. All access to these folders must be done from the longhand path `/maps/...`

## Instructions for using R Singularity images

The instructions assume you have set up your VSCode and are familiar with the
current server structure.

1. Login to head node: `ssh $USER@danhead01fl.unicph.domain`
2. Start a new tmux session: `tmux new -s rsingularity` or attach to a running tmux session `tmux -a`
3. Start a new job: `srun -w {SERVER} -c {CORES} --mem={MEMORY}gb --time={d-H:MM:SS} --pty bash`
4. Load modules: `module load miniconda/latest vscode_cli gcc/11.2.0 singularity/3.8.0`
5. Run command: `code tunnel`
6. Go to your VSCode and on the left panel search for `Remote Explorer`
7. Click `dancmpn01flunicphdom` or `dancmpn02flunicphdom`

Instead of using `.qmd` files, now use `.ipynb` for all your analyses.

In the top right hand corner, you should be able to select kernels beginning with `Singularity-`

## Available kernels

|Kernel name|Description   |
|---|---|
|Singularity-DESeq2   |Differential expression analysis of bulk RNA-seq using DESeq2. Contains plotting libraries EnhancedVolcano and ggpubr   |
|Singularity-Seurat   |Single cell RNA-seq analysis with Seurat v4   |