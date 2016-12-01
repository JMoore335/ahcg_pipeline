## ahcg_pipeline
Variant calling pipeline for genomic data analysis


## Download Virtualbox
https://www.virtualbox.org/wiki/Downloads

## Set up Virtualbox
# Download .ova file
https://da1s119xsxmu0.cloudfront.net/sites/developer/native/nativeappsvm/BaseSpace%20Native%20App%20VM%20(phix%20only)%20v9.ova

#Connect via putty
Host: vagrant@localhost
Password: vagrant
Port: 2222

## Clone Shashi's AHCG Pipeline
git clone https://github.com/shashidhar22/ahcg_pipeline.git

## Requirements: Can be downloaded manually or use git pull origin master

1. [Python3 - version 3.4.1](https://www.python.org/download/releases/3.4.1/)
2. [Trimmomatic - version 0.36](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.36.zip)
3. [Bowtie2 - version 2.2.9](https://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.2.9/)
4. [Picard tools - version 2.6.0](https://github.com/broadinstitute/picard/releases/download/2.6.0/picard.jar)
5. [GATK - version 3.4](https://software.broadinstitute.org/gatk/download/)

## Java install
Tried installing java from source, ran into many issues. Installed via sudo.

sudo apt-get install jre-default

##
java -jar picard.jar CreateSequenceDictionary R=hg.19.fa O=hg19.dict

## Install samtools
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

bowtie2-build -f /path/to/hg19.fa hg19

## Download the reference genome
wget www.prism.gatech.edu/~sravishankar9/resources.tar.gz

gunzip resources.tar.gz

tar -xvf resources.tar

## Download NIST test data

wget ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R1_001.fastq.gz

wget ftp://ftp-trace.ncbi.nih.gov/giab/ftp/data/NA12878/Garvan_NA12878_HG001_HiSeq_Exome/NIST7035_TAAGGCGA_L001_R2_001.fastq.gz

gunzip NIST7035_TAAGGCGA_L001_R1_001.fastq.gz

gunzip NIST7035_TAAGGCGA_L001_R2_001.fastq.gz

head -100000 NIST7035_TAAGGCGA_L001_R1_001.fastq > test_r1.fastq

head -100000 NIST7035_TAAGGCGA_L001_R2_001.fastq > test_r2.fastq

## Run the ahcg pipeline

python3 ahcg_pipeline.py -t ./lib/Trimmomatic-0.36/trimmomatic-0.36.jar -b ./lib/bowtie2-2.2.9/bowtie2 -p ./lib/picard.jar -g ./lib/GenomeAnalysisTK.jar -i ./test_r*.fastq -w ./hg19 -d ./resources/dbsnp/dbsnp_138.hg19.vcf -r ./resources/genome/hg19.fa -a ./lib/Trimmomatic-0.36/adapters/TruSeq2-PE.fa -o outputdir


###GIT SECTION

## git ssh
Added github ssh key 'biol'

## changing remotes
##Move to desired directory
cd ahcg_pipeline/

##List current remotes 
git remote -v

[origin  https://github.com/shashidhar22/ahcg_pipeline.git (fetch)]
[origin  https://github.com/shashidhar22/ahcg_pipeline.git (push)]

##Change my remote from HTTPS to SSH
git remote set-url origin git@github.com:jmoore335/ahcg_pipeline.git

##Check that the remote URL changed
git remote -v

[origin  git@github.com/jmoore335/ahcg_pipeline.git (fetch)]
[origin  git@github.com/jmoore335/ahcg_pipeline.git (push)]

## git ignore
Add the directories you don't wish to commit to git ignore

vim git.ignore

Add .fastq files because of large size.

## git commit instructions
git add.README.md
git commit -m "revisions, should be more detailed"
git config --global user.email jmoore335@gatech.edu
git commit -m "revisions, should be more detailed"
git push origin master

The revised README file has now be committed to github.

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
