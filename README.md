# myfirstrepo
For bioinformatics class: *January 26, 2021*

## Description
[Ben](https://github.com/biobenkj) recommended uploading our VCF filtering exercise to `myfirstrepo` repository as practice. 

We will use **commands** to view and manipulate data, such as:
* `zcat`
* `grep`
* `wc`
* `cut`

## Step 0: Log into the HPC and copy data into your directory
`
qsub -I -l walltime=5:00:00 -l nodes=1:ppn=1

cd /primary/projects/training/session2/
mkdir prahl
cd prahl
cp ../raw_data/chr20.RAW.vcf.gz .
less -S chr20.RAW.vcf.gz
`
---
## Step 1: Filtering variants with low call rates


---
## Step 2: Exclude variants based on allele frequency


---
## Step 3: Exclude individuals based on call rates


---
## Step 4: Exclude individuals based on relatedness


---
## Step 5: Exclude individuals based on their ancestry
