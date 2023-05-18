# RDM decisions

## Naming convesions

- date format: `YYYYMMDD`

## Synchronization

We will have to setup a cron job to perform one-way sync between the `/projects`
folder and `NGS_data` folder. All the analysis will be done on danGPU server,
with no **exceptions**!

After project is done and published, it will be moved to `NGS_data`.

## Template engine

We are currently using [cookiecutter](https://github.com/cookiecutter/cookiecutter)
to generate a [folder structure](https://github.com/brickmanlab/project-template).
There is another alternative called [cruft](https://github.com/cruft/cruft) which
allows us to validate and sync old templates with the latest. I think this could
be a better alternative to cookiecutter.

**Note:** I think cruft sounds very useful if we might change the project template in future. Is it as easy to use as cookiecutter when just routinely creating the project folder (should be easy to use for all members of the lab)?

## Data management catalogue

@SLundregan has already build a prototype for NGS_data folder. We could do the
same for the assays, this way we can make a quick decisions on which assays to
to combine when publishing papers.

Since assays will follow a folder structure, we could just traverse all the
folders and combine `description.yaml` and `metadata.yaml`. Better solution
would be to have a database, but that's an extra complexity on top.

## Data management structure

### Assay

There should be an `Assays` folder that will contain all experimental datasets (raw files and pipeline processed files).
Inside `Assays` there will be subfolders named after a unique ID and the date it was created:

```bash
<Assay-ID>_YYYYMMDD
```

\<Assay-ID> should be easily identifiable. For example:

- NGS assays like ChIP-seq, RNA-seq, ATAC-seq will be named: `NGSXX`, where:
  - NGS will denote that the assay is a NGS experiment.
  - XX is a two digit code identifier that will show what kind of NGS assay it is.  Similar to ICD codes.
- Proteomics assays will be named: `PROTXX`, where:
  - PROT denotes the assay is Mass Spectometry Assay
  - XX is a two digit code identifier that will show what kind of Mass Spectrometry assay it is.
- Multi-omics assays could be named: `OMICSXX`. This are assays that combines different types of OMICS, like CITEseq or RIME-ChIPseq.
- Imaging assays: ??

**Note:** we do not know much about proteomics and mass spec data, neither imaging data.

Examples:

- NGS01_20230101: NGS01 is the identifier for bulk RNAseq analysis. This assay was made on 1st January 2023.
- NGS10_20221202: NGS10 is the ID for ChIPseq.

We could probably reserve ranges of ID for particular type of assays. For example, ATAC-seq and ChIP-seq could belong to the same range category `Chromatin accessibility/binding`, e.g. NGS10-NGS20, while RNA-seq techniques could be NGS00-NGS10.

**Note:** Should the identifiers be more human readable to easily find the correct dataset, e.g. NGS_bulkRNA_20230101? Otherwise a key for the 2 digit identifiers should be stored in a common and easy to find place like Brickmanlab wiki.

#### **Folder Structure**

```bash
<Assay-ID>_20230424
├── description.yaml
├── metadata.yaml
├── pipeline.md
├── processed
└── raw
   ├── fastq
   └── samplesheet.csv
```

- **description.yaml**: longer description of the assay in yaml format.
- **metadata.yaml**: metadata file of the assay describing different keys, such as: assay_ID, owner, date, processing_date, processing_pipeline, processed_by, sample_number, organism, short_description, origin (external or internal).
- **pipeline.md**: description of the pipeline used to process raw data??
- **processed**: folder with results of the preprocessing pipeline. Contents depend on the pipeline used
- **raw**: folder with the raw data.
  - *fastq*:In the case of NGS assays, there should be fastq files.
  - *samplesheet.csv*: file that contains metadata information for the samples. This file is used to run the nf-core pipelines. Ideally, it will also contain a column with info regarding the experimental variables and batches so it can be used for down stream analysis as well.

### Project

There should be another folder called `Projects` that will contain project information and data analysis.

A project may use one or more assays to answer a scientific question. This should be, for example, all the data analysis related
to a publication.

The project folder should be named after a unique identifier, such as:

```bash
<Project-ID>_YYYYMMDD
```

\<Project-ID> might consist on the acronym of the owner of the project folder such as `JARH_20230101`.

**Note:** I think this system might not be ideal, any other suggestion?

#### **Folder structure**

```bash
<Project-ID>_20230424
├── data
│  ├── assays
│  ├── external
│  │  ├── mouse_cell_cycle_genes.json
│  │  └── mouse_cell_cycle_genes.rds
│  └── processed
├── documents
├── notebooks
├── README.md
├── reports
│  └── figures
├── requirements.txt
├── results
└── scripts
```

- **data**: folder that contains symlinks or shortcuts to where the data is, avoiding copying and modification of original files.
- **documents**: folder containing word documents, slides or pdfs related to the project, such as explanations on the data or project, papers, etc.
- **notebooks**: folder containing jupyter notebooks, R markdown or Quarto files with the actual data analysis. Using annotated notebooks is ideal for reproducibility and readability purposes.
- **README.md**:
- **reports**: notebooks can be knitted or transformed into html/docx/pdfs versions ideal for sharing with colleagues and also have a formal report of the data analysis prodecure.
  - *figures*: each knitted document might produce figures. The figures will be saved under a subfolder named after the notebook that created them. This is for provenance purposes so you know which notebook created which images.
- **results**: results from the data analysis, such as tables with differentially expressed genes, enrichment results, etc. These results should be saved under a subfolder named after the notebook that created them. This is for provenance purposes so you know which notebook created which results.
- **scripts**: fodler containing helper scripts needed to run data analysis or reproduce the work of the folder

**Note**: maybe we can make an environment folder or put inside the scripts folder a Dockerfile that should give you an environment where you can reproduce the results of the folder?

**Note2**: shouldn't there be also a project_metadata.yaml here as well? also a project_description.yaml too?
**Note3**: I guess this info would be contained in the README.md? The README/project_description could direct the reader to the the project_metatdata.yaml in the assays folder. Will there be a separate project_metadata.yaml for each dataset used (e.g. if both CHIPseq and RNAseq are used in a given project) or a single project_metadata.yaml with metatadata for all datasets used in the project?
