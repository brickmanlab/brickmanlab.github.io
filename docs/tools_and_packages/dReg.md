# dReg

## 1. Running

### Make dReg bigwigs

If your nascent RNA-seq data is already aligned, bw suitable for use with dReg can be prepared using Danko-Lab [RunOnBamToBigWig](https://github.com/Danko-Lab/RunOnBamToBigWig)

If you have fastq files from PRO-seq, GRO-seq, or CHrO-seq, run the Danko-Lab's [mapping pipeline](https://github.com/Danko-Lab/proseq2.0) using the shared `dReg_dataprep` conda environment

**Example SBATCH script for mapping pipeline**

```bash
#!/bin/bash

#SBATCH --job-name=pro_align
#SBATCH -c 20
#SBATCH --mem=30gb
#SBATCH --time=00-24:00:00
#SBATCH --output=01_proseq_alignment.out
#SBATCH --mail-type=BEGIN,END
#SBATCH --mail-user=YOUR-EMAIL

module load miniconda/latest
source activate dReg_dataprep

PROSEQ=("/maps/projects/dan1/data/Brickman/proseq2.0/proseq2.0.bsh")
GENO=("/scratch/Brickman/references/mus_musculus/ensembl/GRCm38_102/")
RESL=("/maps/projects/dan1/data/Brickman/projects/NAME_DATE/data/external/proseq/")
SAMPLES=("SRX14164616_SRR18010280 SRX14164617_SRR18010278")

for sample in ${SAMPLES}; do
    bash ${PROSEQ} -i ${GENO}bwa \
    -c ${GENO}GRCm38.102.genome \
    -PE --RNA5=R2_5prime --UMI1=6 \
    -O ${RESL} \
    -I ${sample} \
    --thread=20
done
```

### GPU check

Check available GPUs and running processes before using dReg. GPU 0 is reserved for Brickman group

```bash
nvidia-smi
```

### Example dReg script

```bash
#!/bin/bash

#SBATCH --job-name=dREG
#SBATCH -c 30
#SBATCH --mem=30gb
#SBATCH --time=00-24:00:00
#SBATCH --output=01-1_dREG.out
#SBATCH --mail-type=BEGIN,END
#SBATCH --mail-user=YOUR-EMAIL

module load miniconda/latest cuda/11.8-dangpu cudnn/8.6.0-dangpu
source activate dReg

BW=("../data/assays/RNA_INITIAL_DATE/processed/bw/")
RESL=("../results/01/dREG/")
dREG=("/projects/dan1/data/Brickman/dREG/run_dREG.bsh")
MODEL=("/projects/dan1/data/Brickman/dREG/resources/asvm.gdm.6.6M.20170828.rdata")


SAMPLES=("0h_A 0h_B 2h_A 2h_B")

for sample in ${SAMPLES}; do
    bash ${dREG} ${BW}${sample}_sorted_filt_dedup_plus.bw ${BW}${sample}_sorted_filt_dedup_minus.bw \
    ${RESL}${sample}_test ${MODEL} \
    30 0
done
```

## 2. Installation

### Installing dReg

**Note:** Python version in conda env must be 3.8, and R version < 4.0

```bash
cd /maps/projects/dan1/data/Brickman/conda/
module load miniconda/latest
mamba env create -p /projects/dan1/data/Brickman/conda/envs/dReg -f dREG.yml
source activate dReg

cd /maps/projects/dan1/data/Brickman/
git clone https://github.com/Danko-Lab/dREG
cd dREG
make R_dependencies

R
devtools::install_github("CshlSiepelLab/RPHAST")
devtools::install_version("MASS", version="7.3-51.5", repos="https://mirrors.dotsrc.org/cran/")
install.packages("e1071", repos="https://mirrors.dotsrc.org/cran/")
devtools::install_version("randomForest", version="4.6-14", repos="https://mirrors.dotsrc.org/cran/")
quit()

make dreg
mkdir resources
cd resources
wget ftp://cbsuftp.tc.cornell.edu/danko/hub/dreg.models/asvm.gdm.6.6M.20170828.rdata
```

### Installing Rgtsvm

Rgtsvm is required for dReg to use GPU resources

```bash
# make sure in dREG repo and that dReg environment is activated
cd /maps/projects/dan1/data/Brickman/dREG
source activate dReg

R
install.packages(c("bit64", "snow", "SparseM"), repos="https://mirrors.dotsrc.org/cran/")
devtools::install_version("lattice", version="0.20-41", repos="https://mirrors.dotsrc.org/cran/")
install.packages("Matrix", repos="https://mirrors.dotsrc.org/cran/")
quit()
mamba install -c conda-forge boost=1.70.0

mkdir third-party
cd third-party
git clone https://github.com/Danko-Lab/Rgtsvm.git
cd Rgtsvm

module load cuda/11.8-dangpu
module load cudnn/8.6.0-dangpu

R CMD INSTALL --configure-args="--with-boost-home=$CONDA_PREFIX" Rgtsvm
```
