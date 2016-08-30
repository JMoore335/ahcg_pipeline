# ahcg_pipeline
Variant calling pipeline for genomic data analysis

## Requirements

1. [Python3 - version 3.4.1](https://www.python.org/download/releases/3.4.1/)
2. [Trimmomatic - version 0.36](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.36.zip)
3. [Bowtie2 - version 2.2.9](https://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.2.9/)
4. [Picard tools - version 2.6.0](https://github.com/broadinstitute/picard/releases/download/2.6.0/picard.jar)
5. [GATK - version 3.4](https://software.broadinstitute.org/gatk/download/)

## Connect:
Connect via putty
username: vagrant
password: vagrant
port: 2222

## Java install
Tried installing java from source, ran into many issues. Installed via sudo.

sudo apt-get install jre-default

##
java -jar picard.jar CreateSequenceDictionary R=hg.19.fa O=hg19.dict

##
Installed samtools (& bcftools, htslib) via source. 
samtools-1.3.1 bcftools-1.3.1 htslib-1.3.1

cd samtools-1.3.1
make
make prefix=/home/bin install


[Edit .bashrc]
PATH=$PATH:/home/bin
export PATH

## samtools faidx
samtools faidx hg19.fa


## bowtie index
Originally downloaded bowtie index from Shashi, but those were not compatible with the reference file. So the index files were built from bowtie.

## github ssh

## Download NIST test data

wget ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R1_001.fastq.gz

wget ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R2_001.fastq.gz

gunzip NIST7035_TAAGGCGA_L001_R1_001.fastq.gz

gunzip NIST7035_TAAGGCGA_L001_R2_001.fastq.gz

head -100000 NIST7035_TAAGGCGA_L001_R1_001.fastq > test_r1.fastq

head -100000 NIST7035_TAAGGCGA_L001_R2_001.fastq > test_r2.fastq

## git ignore

## Reference genome

Reference genomes can be downloaded from [Illumina iGenomes](http://support.illumina.com/sequencing/sequencing_software/igenome.html)

## Test data

Use the following protocol to download and prepare test dataset from NIST sample NA12878

```{sh}
wget ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R1_001.fastq.gz
wget ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R2_001.fastq.gz
gunzip NIST7035_TAAGGCGA_L001_R1_001.fastq.gz
gunzip NIST7035_TAAGGCGA_L001_R2_001.fastq.gz
head -100000 NIST7035_TAAGGCGA_L001_R1_001.fastq > test_r1.fastq
head -100000 NIST7035_TAAGGCGA_L001_R2_001.fastq > test_r2.fastq
```

## Help

To access help use the following command:

```{sh}
python3 ahcg_pipeline.py -h
```
