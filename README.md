# BioEnv
Bioinformatics Environments

## Ref

#### Homo Sapiens

For hg19

Download from [iGenomes @ illumina](https://support.illumina.com/sequencing/sequencing_software/igenome.html)
```bash
$ wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Homo_sapiens/UCSC/hg19/Homo_sapiens_UCSC_hg19.tar.gz
$ tar -zxvf Homo_sapiens_UCSC_hg19.tar.gz
```

```bash
$ pwd
REF/hg/hg19/Homo_sapiens/UCSC/hg19/Sequence/WholeGenomeFasta

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

```bash
$ mkdir -p ref/hg/hg19/Homo_sapiens/UCSC/hg19_22XYM_order/Sequence/WholeGenomeFasta

$ cd ref/hg/hg19/Homo_sapiens/UCSC/hg19_22XYM_order/Sequence/WholeGenomeFasta

$ cp ~/luod_scratch/ref/hg/hg19/Homo_sapiens/UCSC/hg19/Sequence/WholeGenomeFasta/genome.fa .

$ grep -n '>' genome.fa
1:>chrM
334:>chr1
4985348:>chr2
9849337:>chr3
13809787:>chr4
17632874:>chr5
21251181:>chr6
24673484:>chr7
27856259:>chr8
30783541:>chr9
33607811:>chr10
36318507:>chr11
39018639:>chr12
41695678:>chr13
43999077:>chr14
46146069:>chr15
48196698:>chr16
50003795:>chr17
51627701:>chr18
53189247:>chr19
54371828:>chr20
55632340:>chr21
56594939:>chr22
57621032:>chrX
60726445:>chrY

$ sed -n '1,333p' genome.fa > chrM.fa

$ cat chrM.fa >> genome.fa; rm chrM.fa

$ sed -i '1,333d' genome.fa

$ grep -n '>' genome.fa
1:>chr1
4985015:>chr2
9849004:>chr3
13809454:>chr4
17632541:>chr5
21250848:>chr6
24673151:>chr7
27855926:>chr8
30783208:>chr9
33607478:>chr10
36318174:>chr11
39018306:>chr12
41695345:>chr13
43998744:>chr14
46145736:>chr15
48196365:>chr16
50003462:>chr17
51627368:>chr18
53188914:>chr19
54371495:>chr20
55632007:>chr21
56594606:>chr22
57620699:>chrX
60726112:>chrY
61913585:>chrM

$ du genome.fa
3083776	genome.fa

$ du REF/hg/hg19/Homo_sapiens/UCSC/hg19/Sequence/WholeGenomeFasta/genome.fa
3083776	REF/hg/hg19/Homo_sapiens/UCSC/hg19/Sequence/WholeGenomeFasta/genome.fa
```

Build index for newly ordered genome

```bash
$ module load bowtie2
$ mkdir ../Bowtie2Index
$ nohup bowtie2-build genome.fa ../Bowtie2Index/genome &

$ cd ../Bowtie2Index
$ ln -s ../WholeGenomeFasta/genome.fa .
```

Do the similar thing for hg38

```bash
$ mkdir -p ref/hg/hg38/Homo_sapiens/UCSC/hg38_22XYM_order/Sequence/WholeGenomeFasta

$ cd ref/hg/hg38/Homo_sapiens/UCSC/hg38_22XYM_order/Sequence/WholeGenomeFasta

$ cp ~/luod_scratch/ref/hg/hg38/Homo_sapiens/UCSC/hg38/Sequence/WholeGenomeFasta/genome.fa .

$ grep -n '>' genome.fa
1:>chr1
4979131:>chr2
9823003:>chr3
13788916:>chr4
17593209:>chr5
21223976:>chr6
24640097:>chr7
27827018:>chr8
30729792:>chr9
33497688:>chr10
36173638:>chr11
38875372:>chr12
41540880:>chr13
43828168:>chr14
45969044:>chr15
48008869:>chr16
49815637:>chr17
51480787:>chr18
53088254:>chr19
54260608:>chr20
55549493:>chr21
56483694:>chr22
57500065:>chrX
60620884:>chrY
61765434:>chrM
61765767:>chr1_KI270706v1_random
61769270:>chr1_KI270707v1_random
61769912:>chr1_KI270708v1_random
...
61988540:>chrUn_GL000216v2
61992074:>chrUn_GL000218v1
61995298:>chrEBV

$ sed -i '61765767,$d' genome.fa

$ grep -n '>' genome.fa
1:>chr1
4979131:>chr2
9823003:>chr3
13788916:>chr4
17593209:>chr5
21223976:>chr6
24640097:>chr7
27827018:>chr8
30729792:>chr9
33497688:>chr10
36173638:>chr11
38875372:>chr12
41540880:>chr13
43828168:>chr14
45969044:>chr15
48008869:>chr16
49815637:>chr17
51480787:>chr18
53088254:>chr19
54260608:>chr20
55549493:>chr21
56483694:>chr22
57500065:>chrX
60620884:>chrY
61765434:>chrM
```

Build index for newly ordered genome

```bash
$ module load bowtie2
$ mkdir ../Bowtie2Index
$ nohup bowtie2-build genome.fa ../Bowtie2Index/genome &

$ cd ../Bowtie2Index
$ ln -s ../WholeGenomeFasta/genome.fa .
```
