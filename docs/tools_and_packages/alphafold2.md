# Alphafold 2

## 1. Running

### 1.1 Create a target file

```bash
# cat target.fasta
>query
MAAHKGAEHHHKAAEHHEQAAKHHHAAAEHHEKGEHEQAAHHADTAYAHHKHAEEHAAQAAKHDAEHHAPKPH
```

### 1.2. Setup environments

```bash
srun -N 1 --ntasks-per-node=10 --gres=gpu:2 --pty bash
module load miniconda/latest cuda/11.4 cudnn/8.2.2
source activate /maps/projects/dan1/data/Brickman/conda/envs/af2

cd /maps/projects/dan1/data/Brickman/alphafold
export AF2_DATA_DIR="~/projects/data/Alphafold2/24022023"
```

### 1.3. Run monomer (cli)

```bash
python run_alphafold.py \
    --fasta_paths=~/projects/data/Brickman/target_01.fasta \
    --output_dir=/scratch/tmp/alphatest \
    --model_preset=monomer \
    --db_preset=full_dbs \
    --data_dir=$AF2_DATA_DIR \
    --uniref30_database_path=$AF2_DATA_DIR/uniref30/UniRef30_2021_03 \
    --uniref90_database_path=$AF2_DATA_DIR/uniref90/uniref90.fasta \
    --mgnify_database_path=$AF2_DATA_DIR/mgnify/mgy_clusters_2022_05.fa \
    --pdb70_database_path=$AF2_DATA_DIR/pdb70/pdb70 \
    --template_mmcif_dir=$AF2_DATA_DIR/pdb_mmcif/mmcif_files/ \
    --obsolete_pdbs_path=$AF2_DATA_DIR/pdb_mmcif/obsolete.dat \
    --bfd_database_path=$AF2_DATA_DIR/bfd/bfd_metaclust_clu_complete_id30_c90_final_seq.sorted_opt \
    --max_template_date=2022-01-01 \
    --use_gpu_relax
```

### 1.4. Run multimer (cli)

The example below generates **10 models**.

```bash
python run_alphafold.py \
    --fasta_paths=/home/fdb589/projects/data/Brickman/WTPU_1_WTC_EBPa.fasta \
    --output_dir=/scratch/tmp/alphatest \
    --model_preset=multimer \
    --db_preset=full_dbs \
    --data_dir=$AF2_DATA_DIR \
    --uniref30_database_path=$AF2_DATA_DIR/uniref30/UniRef30_2021_03 \
    --uniref90_database_path=$AF2_DATA_DIR/uniref90/uniref90.fasta \
    --mgnify_database_path=$AF2_DATA_DIR/mgnify/mgy_clusters_2022_05.fa \
    --template_mmcif_dir=$AF2_DATA_DIR/pdb_mmcif/mmcif_files/ \
    --obsolete_pdbs_path=$AF2_DATA_DIR/pdb_mmcif/obsolete.dat \
    --pdb_seqres_database_path=$AF2_DATA_DIR/pdb_seqres/pdb_seqres.txt \
    --uniprot_database_path=$AF2_DATA_DIR/uniprot/uniprot.fasta \
    --bfd_database_path=$AF2_DATA_DIR/bfd/bfd_metaclust_clu_complete_id30_c90_final_seq.sorted_opt \
    --max_template_date=2022-01-01 \
    --num_multimer_predictions_per_model=10 \
    --use_gpu_relax
```

### 1.5. Example SBATCH script

```bash
#!/bin/bash
#SBATCH --job-name=AF2
#SBATCH --gres=gpu:2
#SBATCH --cpus-per-task=10
#SBATCH --mail-type=BEGIN,END
#SBATCH --mail-user=YOUR-EMAIL

module load miniconda/latest cuda/11.4 cudnn/8.2.2
source activate /maps/projects/dan1/data/Brickman/conda/envs/af2
cd ~/projects/data/Brickman/alphafold
mkdir -p /scratch/tmp/alphatest
export AF2_DATA_DIR="~/projects/data/Alphafold2/24022023"

srun python run_alphafold.py \
--fasta_paths=~/projects/data/Brickman/target_01.fasta \
--output_dir=/scratch/tmp/alphatest \
--model_preset=monomer \
--db_preset=full_dbs \
--data_dir=$AF2_DATA_DIR \
--uniref30_database_path=$AF2_DATA_DIR/uniref30/UniRef30_2021_03 \
--uniref90_database_path=$AF2_DATA_DIR/uniref90/uniref90.fasta \
--mgnify_database_path=$AF2_DATA_DIR/mgnify/mgy_clusters_2022_05.fa \
--pdb70_database_path=$AF2_DATA_DIR/pdb70/pdb70 \
--template_mmcif_dir=$AF2_DATA_DIR/pdb_mmcif/mmcif_files/ \
--obsolete_pdbs_path=$AF2_DATA_DIR/pdb_mmcif/obsolete.dat \
--bfd_database_path=$AF2_DATA_DIR/bfd/bfd_metaclust_clu_complete_id30_c90_final_seq.sorted_opt \
--max_template_date=2022-01-01 \
--use_gpu_relax
```

## 2. Installation

```bash
conda create --prefix /maps/projects/dan1/data/Brickman/conda/envs/af2 python=3.8
source activate /maps/projects/dan1/data/Brickman/conda/envs/af2

mamba install hmmer
pip install py3dmol
mamba install pdbfixer==1.7
mamba install -c conda-forge openmm=7.5.1

cd /maps/projects/dan1/data/Brickman/
git clone --branch main https://github.com/deepmind/alphafold alphafold
pip install -r ./alphafold/requirements.txt
pip install --no-dependencies ./alphafold

# stereo chemical props needs to be in common folder
wget –q –P  /maps/projects/dan1/data/Brickman/alphafold/alphafold/common/ https://git.scicore.unibas.ch/schwede/openstructure/-/raw/7102c63615b64735c4941278d92b554ec94415f8/modules/mol/alg/src/stereo_chemical_props.txt

# skipping content part
mkdir -p ./alphafold/data/params && cd ./alphafold/data/params
wget https://storage.googleapis.com/alphafold/alphafold_params_colab_2022-12-06.tar
tar --extract --verbose --preserve-permissions --file alphafold_params_colab_2022-12-06.tar
pip install ipykernel ipywidgets tqdm
pip install --upgrade scprep phate

# Install jax
module load miniconda/latest
module load cuda/11.4 cudnn/8.2.2
export CUDA_VISIBLE_DEVICES='3'
pip install "jax[cuda11_cudnn82]" -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html

# fix last issues
mamba install -c conda-forge -c bioconda hhsuite
mamba install -c bioconda kalign3
pip install numpy==1.21.6
```

### 2.1. Download references

!!! note

    Downloading references will not work on one try, had to do a lot of manual
    re-running of scripts.

```bash
# create folder
mkdir -p ~/projects/data/Alphafold2/24022023
cd ~/projects/data/Alphafold2/24022023

# Download all databases
sh download_all_data.sh ~/projects/data/Alphafold2/24022023/ > download.log 2> download_all.log

# Some fix-ups
# mmCIF will not work because the firewall blocks the port, so I found this workaroud online
# ref: https://github.com/deepmind/alphafold/issues/196
wget -e robots=off -r --no-parent -nH --cut-dirs=7 -q ftp://ftp.ebi.ac.uk/pub/databases/pdb/data/structures/divided/mmCIF/ -P "${RAW_DIR}"

# Last step is to fix all the permissions
chmod -R 755 24022023/
```

## References

- [ifb-elixirfr](https://ifb-elixirfr.gitlab.io/cluster/doc/software/alphafold2)
- [deepmind/alphafold](https://github.com/deepmind/alphafold)
