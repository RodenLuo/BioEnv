# BioEnv
Bioinformatics Environments

## Ref

#### Homo Sapiens

For hg19

Download from iGenomes @ illumina
```bash
$ wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Homo_sapiens/UCSC/hg19/Homo_sapiens_UCSC_hg19.tar.gz
$ tar -zxvf Homo_sapiens_UCSC_hg19.tar.gz
```

```bash
$ pwd
./ref/hg/hg19/Homo_sapiens/UCSC/hg19/Sequence/WholeGenomeFasta

$ grep '>' genome.fa
>chrM
>chr1
>chr2
>chr3
>chr4
>chr5
>chr6
>chr7
>chr8
>chr9
>chr10
>chr11
>chr12
>chr13
>chr14
>chr15
>chr16
>chr17
>chr18
>chr19
>chr20
>chr21
>chr22
>chrX
>chrY
```

According to this GATK's [page](https://gatkforums.broadinstitute.org/gatk/discussion/1204/what-input-files-does-the-gatk-accept-require), this is the canonical order of hg1x reference. But in hg38, it seems that UCSC changed the chrM to the last following chrY. So I made another version which put chrM as the last, everthing else unchanged.

