# Starting a new assay or project

Whenever you obtain sequencing data from Genomic's Platform, you have to
create an **Assay**. By running the commands below, you will have option to
fill all required information about the experiment. This workflow will help
us with tracking of all sequencing done in our lab.

## Assay

When you sequence an experiment, we create an **Assay** out of it, so we can use
it in a project afterwards.

Login to danhead and run command:

```bash
create_assay
```

## Project

Every time you want to make some analysis, you should create a **project**. Our
folder structure will allow you to easily link various **experiments** to your
project and make your analysis easier.

Please use the following naming convention: `surname-<YOUR_CODENAME>`

```bash
create_project
```

Link required assays to your project.

```bash
ln -s /maps/projects/dan1/data/Brickman/assays/<ASSAY_ID> /maps/projects/dan1/data/Brickman/projects/<PROJECT_ID>/data/assays/
```

Link **external data** if needed

```bash
ln -s /maps/projects/dan1/data/Brickman/shared /maps/projects/dan1/data/Brickman/projects/<PROJECT_ID>/data/external/
```
