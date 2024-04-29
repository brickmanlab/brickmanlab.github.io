# UCSC liftover tool

[UCSC liftover](https://genome.ucsc.edu/cgi-bin/hgLiftOver)

## Issue: separate peaks map to same coordinates after liftover

- When using the tool to liftover regions from hg19 to hg38, separate peaks in specific regions of hg19 map to overlapping coordinates after liftover.
- This is because some contigs were not carried forward from hg19 to hg38, due to problems with the hg19 assembly that were resolved in hg38
- All lifted over peaks with overlapping coordinates should be removed from analysis because the peaks were called on an uncertain region in the old genome assembly
- Issues like this likely apply to all genome updates, not only human
- The same issue also applies with [Ensembl assembly converter](https://www.ensembl.org/Homo_sapiens/Tools/AssemblyConverter) 

## Solution
Remove any peaks with overlapping coordinates after liftover before using the lifted over peak file:

```bash
#!/bin/bash

module load bedtools

EXTL=("../data/external/")

# Sort the lifted over peakfile for use with bedtools
sort -k1,1 -k2,2n ${EXTL}wong_fig3c_peaks_GRCh38.bed > ${EXTL}peaks.tmp && mv ${EXTL}peaks.tmp ${EXTL}wong_fig3c_peaks_GRCh38.bed

# Bedtools merge count rows contributing to merged peaks (overlapping peaks will have count > 1)
bedtools merge -i ${EXTL}wong_fig3c_peaks_GRCh38.bed -c 1 -o count > ${EXTL}counted.bed

# Get non-overlapping peaks
awk '/\t1$/{print}' ${EXTL}counted.bed > ${EXTL}filtered.bed

# Intersect original file with non-overlapping peaks and output overlapping peaks
bedtools intersect -wa -a ${EXTL}wong_fig3c_peaks_GRCh38.bed -b ${EXTL}filtered.bed > ${EXTL}wong_fig3c_peaks_GRCh38_correct_liftover.bed
bedtools intersect -v -a ${EXTL}wong_fig3c_peaks_GRCh38.bed -b ${EXTL}filtered.bed > ${EXTL}wong_fig3c_peaks_GRCh38_overlapping.bed
```
