# myfirstrepo
For bioinformatics class: *January 26, 2021*

**Note** I'm basically only writing this part as a refresher for using markdown syntax.

## Description
[Ben](https://github.com/biobenkj) recommended uploading our VCF filtering exercise to `myfirstrepo` repository as practice. 

We will use **commands** to view and manipulate data, such as:
* `zcat`
* `grep`
* `wc`
* `cut`

## Step 0: Log into the HPC and copy data into your directory
```
qsub -I -l walltime=5:00:00 -l nodes=1:ppn=1   
cd /primary/projects/training/session2/  
mkdir prahl   
cd prahl   
cp ../raw_data/chr20.RAW.vcf.gz .   
less -S chr20.RAW.vcf.gz   
```
**Questions**
1. What chromosome are we working on? **chr20**
2. The number of variants? `zcat chr20.RAW.vcf.gz | grep -v '^#' | cut -1 | sort | uniq -c`= **49305 variants**
3. The number of individuals? `zcat chr20.RAW.vcf.gz | grep '^#CHROM' | cut -10- | wc -w` = **108 individuals**

---
## Step 1: Filtering variants with low call rates
```
module load bbc/vcftools/vcftools-0.1.16
module load bbc/htslib/htslib-1.10.2
module load bbc/R/R-3.6.0/

Rscript ../Rscripts/step1.Rscript

vcftools --gzvcf chr20.RAW.vcf.gz --max-missing 0.1 --recode --stdout | bgzip -c > chr20.step1.vcf.gz
tabix -p vcf chr20.step1.vcf.gz
```

**Question:** How many variants have been removed? **30 variants removed**

---
## Step 2: Exclude variants based on allele frequency
```
vcftools --gzvcf ../raw_data/reference/chr20.EUR.vcf.gz --freq2 --stdout|sed '1d'|awk '{print $2,$4,$5,$6}' > chr20.EUR.freq
vcftools --gzvcf chr20.step1.vcf.gz --freq2 --stdout | sed '1d' | awk '{print $2,$4,$5,$6}' > chr20.step.freq
Rscript ../Rscripts/step2.Rscript 
vcftools --gzvcf chr20.step1.vcf.gz --exclude-positions chr20.step1.filtered.txt --recode --stdout|bgzip -c > chr20.step2.vcf.gz
```


**Question:** How many variants are filtered out?

---
## Step 3: Exclude individuals based on call rates
```
vcftools --gzvcf chr20.step2.vcf.gz --missing-indv --stdout > chr20.step2.missing.tsv

vcftools --gzvcf chr20.step2.vcf.gz --remove chr20.step3.removeIndividuals.tsv --recode --stdout | bgzip -c > chr20.step3.vcf.gz
```

**Questions**
1. What does each line in chr20.step2.missing.tsv represent?
2. Can you sort this file based on the missing data rate?
3. What individual has the highest missing data rate?
4. How many individuals have a missing rate greater than 2%?
5. Confirm that you excluded the individuals from the new file using zcat, cut, and wc. What command did you use?

---
## Step 4: Exclude individuals based on relatedness


---
## Step 5: Exclude individuals based on their ancestry
