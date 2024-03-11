---
title: "bash one liner"
author: ''
date: "2023-11-16"
excerpt: ''
slug: []
categories: ["bash"]
tags: ["one liner"]
---


Just some bash notes which i find very useful but i keep forgetting.

# Find 


```bash
# find and grep
find . -name "*.Rmd" | xargs grep "enricher"

# find and execute
find . -name "*rawcounts.csv" -exec scp -r -i ~/.ssh/id_transfer {} transfer@pmcc-transfer-server.petermac.org.au:/data/transfer/antonio.ahn/raw_counts_csv \;

# find multiple files in a directory
find $bam_files_dir \( -name "*MDA-MB-453*_unique_aln.bam" -o -name "*M453*_unique_aln.bam" \)
# then use that to merge 
samtools merge m453_DMSO_7D_merged.bam $(find $bam_files_dir -name "*MDA-MB-453*_unique_aln.bam" | grep -E 'DMSO_7d|LY_7d|LYR1000')
```






# xarg


```bash
# copy pasting all files except for the 3 named into a different directory
ls  | grep -v -E '0.choosing_strand|subread|archived' | xargs -I {} cp -r "{}" /new/directory
```

# xarg 2

from /researchers/antonio.ahn/Goel_Labmembers/CatherinePiggin/2021/5.CutandTag_LY7DvsDMSO_July2021/workflow

```bash
for i in H3K27ac H3K4me1 H3K9me3 H3K27me3;
do
array=($(ls $SEACR_dir/*${i}*.bed))
first_sample_names=$(echo ${array[0]} | awk -F '/' '{print $NF}')
rest_sample_names=$(printf '%s\n' "${array[@]:1}" | awk -F '/' '{print $NF}')
echo "first sample names are $first_sample_names"
echo "second sample names are $rest_sample_names" 
bedtools intersect -wa -wb -a ${array[0]} -b ${array[@]:1} > $output_dir/intersect_test_${i}.bed;

done
```


# sample names 


```bash
# For ATAC peaks (LYR minus DMSO)
for i in H3K27ac H3K4me1 H3K9me3 H3K27me3;
do
sample_names=`ls ${BWpath_cutandtag}/*${i}*.bw | awk -F '/' '{print $NF}' | awk -F '-' 'OFS="_" {print $3,$4,$5}'`
bigwig_names=`ls ${BWpath_cutandtag}/*${i}*.bw`

echo "sample names are ${sample_names}"
echo "bigwig names are ${bigwig_names}"

computeMatrix reference-point --referencePoint center \
-b 2000 -a 2000 \
-R $BEDpath/merged_${i}.bed \
-S `ls $BWpath_cutandtag/*${i}*.bw` \
--skipZeros \
--missingDataAsZero \
--sortRegions descend \
-p 11 \
-o $output_path/cutandtag_${i}_matrix.gz

plotHeatmap -m  $output_path/cutandtag_${i}_matrix.gz \
-out $output_path/cutandtag_${i}.png  \
--colorMap Blues \
--dpi 300 \
--refPointLabel center \
--samplesLabel $sample_names \
--yAxisLabel "${i} enrichment" \
--regionsLabel merged.bed;
done
```


# matching different patterns for listing files 


```r
# This matches file names with either DMSO_7d.*bam or LY_7d.*bam$inside the name and both ends with .*bam or LY_7d.bam
bamReads_1 <- file.path("/researchers/antonio.ahn/1.BostonData_August2020/1.ATACseq/results_250820_set1", list.files(path = "/researchers/antonio.ahn/1.BostonData_August2020/1.ATACseq/results_250820_set1", pattern = "DMSO_7d.*bam$|LY_7d.*bam$"))
```


```bash
# But in bash you do this 
ls *{DMSO_7d,LY_7d}*.bam

# For gsub i also used s* (at https://stackoverflow.com/questions/17166618/regular-expression-containing-one-word-or-another)

```



# print only a subset or make array 

file names are 001-sample-treated-TF1.bam, 002-sample-treated-TF1.bam .. 017-sample-treated-TF1.bam

```bash
echo $(printf "%03d-MCF7-*.bam " {1..17})
```

```
## 001-MCF7-*.bam 002-MCF7-*.bam 003-MCF7-*.bam 004-MCF7-*.bam 005-MCF7-*.bam 006-MCF7-*.bam 007-MCF7-*.bam 008-MCF7-*.bam 009-MCF7-*.bam 010-MCF7-*.bam 011-MCF7-*.bam 012-MCF7-*.bam 013-MCF7-*.bam 014-MCF7-*.bam 015-MCF7-*.bam 016-MCF7-*.bam 017-MCF7-*.bam
```

