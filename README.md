# comts
## Comts: A pipeline for calculating single-copy genes’ community abundance in metagenome
## Introduction
Gene abundance in metagenome datasets is commonly represented in terms of Reads Per Kilobase per Million Reads (RPKM), Fragments Per Kilobase per Million (FPKM) and Transcripts Per Million (TPM). However, the gene abundance in microbial community (GAM,%), defined as the proportion of microorganisms containing the gene to the overall population, remains underexplored and lacks a standardized methodology for estimation. In this study, we introduce Comts, a comprehensive framework for estimating GAM, and present a robust, user-friendly and efficient computational pipeline designed to calculate GAM from metagenomic sequencing data. The developed pipeline makes it accessible to researchers seeking to evaluate the metabolic capabilities of microbial communities, particularly for single-copy genes with specific metabolic function.
## The formula
GAM =  (RSCG×100%)/MRUSCG

## Download and Installation
### The softwares listed below must have been installed before installation :robot:
> diamond  
> seqkit  
> fastp
### The R packages listed below must have been installed before installation  
> optparse
> dplyr
> tidyr
> data.table
> magrittr
> ggplot2
### Download throught `git clone` :
`git clone https://github.com/XiangZhouCAS/comts.git`
### Installation
1. `sh ./install.sh`
2. `source ~/.bashrc`
### DataBase
Ribo_14.dmnd  
hyddb_all.dmnd (Søndergaard, D., Pedersen, C. & Greening, C. HydDB: A web tool for hydrogenase classification and analysis. Sci Rep 6, 34212 (2016). https://doi.org/10.1038/srep34212)  
ter.dmnd.gz ([Hydrogen metabolism terminal enzyme's database providede by GreeningLab](https://github.com/GreeningLab/GreeningLab-database/blob/main/Original%20database%20(2020)))
## Usage
| Function | Description |
|-------|-------|
|`comts geneset`|To calculate GAM of single copy genes through GeneSet.|
|`comts custom`|To calculate GAM abandance of single copy genes through custom database.|

## Parameter details
### comts geneset ribo
- `comts geneset ribo` To calculate RPKM abandance of 14 universal single copy ribosomal genes.  
| Parameter | Description |
|-------|-------|
|`--input_reads` `-i`|Set the directory of reads.|
|`--result` `-o`|Set the result file name.|
|`--threads` `-t`|Set the threads of CPU,default is 1.|
|`--UCSG_db` `-u`|Set the directory of universal single copy genes (USCGs) database.|
|`--skip_fastp` `-s`|If you have already filtered the reads, you can set this parameter to skip running fastp. The default is to run fastp.|
|`--min_length` `-m`|Set the minimum length required for filtering reads, the default is 100, but it is recommended to set this parameter to 140 if hydrogenases or hydrogen metabolism terminal enzymes are to be calculated.|
|`--run_seqkit` `-k`|If you have already counted the total number of reads using seqkit, you can specify the directory of the seqkit results (e.g., 'sample_name.all.reads.txt') to skip running seqkit. By default, seqkit will be executed.|
|`--keep_samples` `-e`|By default, the temporary results will be deleted unless this parameter is setted.|
|`--help` `-h`|Show the help message and exit.|
```
comts geneset ribo -i sample1.1.fastq.gz -o sample1 -t 4 -s Ribo_14.dmnd --min_length 140
```
- `comts geneset res` To convert RPKM to community abandance of single copy function gene through GeneSet.  
```
comts geneset res -i geneset.rpkm.txt -r Ribo.rpkm.txt -o community.abd.txt
```
### comts custom
- `comts custom diy` To calculate single copy function enzyme gene community abandance by custom database.
```
comts custom diy -i sample1.1.fastq.gz -o sample1 -t 4 -d function_genes.dmnd -s Ribo_14.dmnd
```
- `comts custom ter` To calculate single copy terminal enzyme gene community abandance.  
```
comts custom ter -i sample1.1.fastq.gz -o sample1 -t 4 -d terminal_genes.dmnd -s Ribo_14.dmnd
```
- `comts custom hyd` To calculate single copy Hydrogenase community abandance.
```
comts custom hyd -i sample1.1.fastq.gz -o sample1 -t 4 -d hyddb.all.dmnd -s Ribo_14.dmnd -c hyd_id-name.script
```
