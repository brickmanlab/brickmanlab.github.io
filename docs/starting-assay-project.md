# Starting a new assay or project

Whenever you obtain sequencing data from Genomic's Platform, you have to
create an **Assay**. By running the commands below, you will have option to
fill all required information about the experiment. This workflow will help
us with tracking of all sequencing done in our lab.

## Assay

```bash
module load miniconda/latest
source activate brickman

cd /home/$USER/Brickman/assays
cruft create https://github.com/brickmanlab/ngs-template --directory="assay"
```

Next, please copy all `fastq` files from CPR share to folder `data/raw/fastq`.
All processed data from a pipeline should be copied to `data/processed` folder.

```bash
rsync -avzh --progress --chmod=2775 ~/ucph/ndir/SUN-CPR-genomics_data/... <ASSAY_ID>/data/raw/fastq
```

After copying all the files, please run the last command.

```bash
chmod -R 775 /maps/projects/dan1/data/Brickman/assays/<ASSAY_ID>
```

## Project

Every time you want to make some analysis, you should create a **project**. Our
folder structure will allow you to easily link various **experiments** to your
project and make your analysis easier.

Please use the following naming convention: `surname-<YOUR_CODENAME>`

```bash
module load miniconda/latest
source activate brickman

cd /home/$USER/Brickman/projects
cruft create https://github.com/brickmanlab/ngs-template --directory="project"
```

Link required assays to your project.

```bash
ln -s /maps/projects/dan1/data/Brickman/assays/<ASSAY_ID> /maps/projects/dan1/data/Brickman/projects/<PROJECT_ID>/data/assays/
```

Link external data if needed

```bash
ln -s /maps/projects/dan1/data/Brickman/shared /maps/projects/dan1/data/Brickman/projects/<PROJECT_ID>/data/external/
```
