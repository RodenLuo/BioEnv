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

Download and extract the NCBI GRCh38 and build37.2.
```bash
$ wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Homo_sapiens/NCBI/GRCh38/Homo_sapiens_NCBI_GRCh38.tar.gz
$ wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Homo_sapiens/NCBI/build37.2/Homo_sapiens_NCBI_build37.2.tar.gz

$ nohup tar -zxvf Homo_sapiens_NCBI_GRCh38.tar.gz &
$ nohup tar -zxvf Homo_sapiens_NCBI_build37.2.tar.gz &
```

Do the same for NCBI's as UCSC's.

For NCBI_build37.2

```bash
$ grep -n '^>' genome.fa
1:>1
3560726:>2
7035004:>3
9863897:>4
12594674:>5
15179179:>6
17623681:>7
19897092:>8
21988008:>9
24005344:>10
25941556:>11
27870222:>12
29782393:>13
31427678:>14
32961244:>15
34425980:>16
35716764:>17
36876697:>18
37992088:>19
38836789:>20
39737155:>21
40424726:>22
41157650:>X
43375802:>Y
44223997:>MT
```
Already good. **But Pay attention to the numbering system. 1-22 rather than chr1-chr22, MT rather than chrM.**

Check GRCh38.
```bash
$ grep -n '^>' genome.fa
1:>chr1  AC:CM000663.2  gi:568336023  LN:248956422  rl:Chromosome  M5:6aef897c3d6ff0c78aff06ac189178dd  AS:GRCh38
3556523:>chr2  AC:CM000664.2  gi:568336022  LN:242193529  rl:Chromosome  M5:f98db672eb0993dcfdabafe2a882905c  AS:GRCh38
7016432:>chr3  AC:CM000665.2  gi:568336021  LN:198295559  rl:Chromosome  M5:76635a41ea913a405ded820447d067b0  AS:GRCh38
9849227:>chr4  AC:CM000666.2  gi:568336020  LN:190214555  rl:Chromosome  M5:3210fecf1eb92d5489da4346b3fddc6e  AS:GRCh38
12566579:>chr5  AC:CM000667.2  gi:568336019  LN:181538259  rl:Chromosome  M5:a811b3dc9fe66af729dc0dddf7fa4f13  AS:GRCh38  hm:47309185-49591369
15159984:>chr6  AC:CM000668.2  gi:568336018  LN:170805979  rl:Chromosome  M5:5691468a67c7e7a7b5f2a3a683792c29  AS:GRCh38
17600071:>chr7  AC:CM000669.2  gi:568336017  LN:159345973  rl:Chromosome  M5:cc044cc2256a1141212660fb07b6171e  AS:GRCh38
19876444:>chr8  AC:CM000670.2  gi:568336016  LN:145138636  rl:Chromosome  M5:c67955b5f7815a9a1edfaa15893d3616  AS:GRCh38
21949855:>chr9  AC:CM000671.2  gi:568336015  LN:138394717  rl:Chromosome  M5:6c198acf68b5af7b9d676dfdd531b5de  AS:GRCh38
23926924:>chr10  AC:CM000672.2  gi:568336014  LN:133797422  rl:Chromosome  M5:c0eeee7acfdaf31b770a509bdaa6e51a  AS:GRCh38
25838317:>chr11  AC:CM000673.2  gi:568336013  LN:135086622  rl:Chromosome  M5:1511375dc2dd1b633af8cf439ae90cec  AS:GRCh38
27768127:>chr12  AC:CM000674.2  gi:568336012  LN:133275309  rl:Chromosome  M5:96e414eace405d8c27a6d35ba19df56f  AS:GRCh38
29672061:>chr13  AC:CM000675.2  gi:568336011  LN:114364328  rl:Chromosome  M5:a5437debe2ef9c9ef8f3ea2874ae1d82  AS:GRCh38
31305839:>chr14  AC:CM000676.2  gi:568336010  LN:107043718  rl:Chromosome  M5:e0f0eecc3bcab6178c62b6211565c807  AS:GRCh38  hm:multiple
32835036:>chr15  AC:CM000677.2  gi:568336009  LN:101991189  rl:Chromosome  M5:f036bd11158407596ca6bf3581454706  AS:GRCh38
34292054:>chr16  AC:CM000678.2  gi:568336008  LN:90338345  rl:Chromosome  M5:db2d37c8b7d019caaf2dd64ba3a6f33a  AS:GRCh38
35582603:>chr17  AC:CM000679.2  gi:568336007  LN:83257441  rl:Chromosome  M5:f9a0fb01553adb183568e3eb9d8626db  AS:GRCh38
36771997:>chr18  AC:CM000680.2  gi:568336006  LN:80373285  rl:Chromosome  M5:11eeaa801f6b0e2e36a1138616b8ee9a  AS:GRCh38
37920188:>chr19  AC:CM000681.2  gi:568336005  LN:58617616  rl:Chromosome  M5:85f9f4fc152c58cb7913c06d6b98573a  AS:GRCh38  hm:multiple
38757584:>chr20  AC:CM000682.2  gi:568336004  LN:64444167  rl:Chromosome  M5:b18e6c531b0bd70e949a7fc20859cb01  AS:GRCh38
39678216:>chr21  AC:CM000683.2  gi:568336003  LN:46709983  rl:Chromosome  M5:974dc7aec0b755b19f031418fdedf293  AS:GRCh38  hm:multiple
40345503:>chr22  AC:CM000684.2  gi:568336002  LN:50818468  rl:Chromosome  M5:ac37ec46683600f808cdd41eac1d55cd  AS:GRCh38  hm:multiple
41071483:>chrX  AC:CM000685.2  gi:568336001  LN:156040895  rl:Chromosome  M5:2b3a55ff7f58eb308420c8a9b11cac50  AS:GRCh38
43300640:>chrY  AC:CM000686.2  gi:568336000  LN:57227415  rl:Chromosome  M5:ce3e31103314a704255f3cd90369ecce  AS:GRCh38  hm:10001-2781479,56887903-57217415
44118176:>chrM  AC:J01415.2  gi:113200490  LN:16569  rl:Mitochondrion  M5:c68f52674c9fb33aef52dcf399755519  AS:GRCh38  tp:circular
44118414:>chr1_KI270706v1_random  AC:KI270706.1  gi:568335410  LN:175055  rg:chr1  rl:unlocalized  M5:62def1a794b3e18192863d187af956e6  AS:GRCh38
44120916:>chr1_KI270707v1_random  AC:KI270707.1  gi:568335409  LN:32032  rg:chr1  rl:unlocalized  M5:78135804eb15220565483b7cdd02f3be  AS:GRCh38
44121375:>chr1_KI270708v1_random  AC:KI270708.1  gi:568335408  LN:127682  rg:chr1  rl:unlocalized  M5:1e95e047b98ed92148dd84d6c037158c  AS:GRCh38
44123201:>chr1_KI270709v1_random  AC:KI270709.1  gi:568335407  LN:66860  rg:chr1  rl:unlocalized  M5:4e2db2933ea96aee8dab54af60ecb37d  AS:GRCh38
44124158:>chr1_KI270710v1_random  AC:KI270710.1  gi:568335406  LN:40176  rg:chr1  rl:unlocalized  M5:9949f776680c6214512ee738ac5da289  AS:GRCh38
44124733:>chr1_KI270711v1_random  AC:KI270711.1  gi:568335405  LN:42210  rg:chr1  rl:unlocalized  M5:af383f98cf4492c1f1c4e750c26cbb40  AS:GRCh38
44125337:>chr1_KI270712v1_random  AC:KI270712.1  gi:568335404  LN:176043  rg:chr1  rl:unlocalized  M5:c38a0fecae6a1838a405406f724d6838  AS:GRCh38
44127853:>chr1_KI270713v1_random  AC:KI270713.1  gi:568335403  LN:40745  rg:chr1  rl:unlocalized  M5:cb78d48cc0adbc58822a1c6fe89e3569  AS:GRCh38
44128437:>chr1_KI270714v1_random  AC:KI270714.1  gi:568335402  LN:41717  rg:chr1  rl:unlocalized  M5:42f7a452b8b769d051ad738ee9f00631  AS:GRCh38
44129034:>chr2_KI270715v1_random  AC:KI270715.1  gi:568335401  LN:161471  rg:chr2  rl:unlocalized  M5:b65a8af1d7bbb7f3c77eea85423452bb  AS:GRCh38
44131342:>chr2_KI270716v1_random  AC:KI270716.1  gi:568335400  LN:153799  rg:chr2  rl:unlocalized  M5:2828e63b8edc5e845bf48e75fbad2926  AS:GRCh38
44133541:>chr3_GL000221v1_random  AC:GL000221.1  gi:224183270  LN:155397  rg:chr3  rl:unlocalized  M5:3238fb74ea87ae857f9c7508d315babb  AS:GRCh38
44135762:>chr4_GL000008v2_random  AC:GL000008.2  gi:568335399  LN:209709  rg:chr4  rl:unlocalized  M5:a999388c587908f80406444cebe80ba3  AS:GRCh38
44138759:>chr5_GL000208v1_random  AC:GL000208.1  gi:224183050  LN:92689  rg:chr5  rl:unlocalized  M5:aa81be49bf3fe63a79bdc6a6f279abf6  AS:GRCh38
44140085:>chr9_KI270717v1_random  AC:KI270717.1  gi:568335398  LN:40062  rg:chr9  rl:unlocalized  M5:796773a1ee67c988b4de887addbed9e7  AS:GRCh38
44140659:>chr9_KI270718v1_random  AC:KI270718.1  gi:568335397  LN:38054  rg:chr9  rl:unlocalized  M5:b0c463c8efa8d64442b48e936368dad5  AS:GRCh38
44141204:>chr9_KI270719v1_random  AC:KI270719.1  gi:568335396  LN:176845  rg:chr9  rl:unlocalized  M5:cd5e932cfc4c74d05bb64e2126873a3a  AS:GRCh38
44143732:>chr9_KI270720v1_random  AC:KI270720.1  gi:568335395  LN:39050  rg:chr9  rl:unlocalized  M5:8c2683400a4aeeb40abff96652b9b127  AS:GRCh38
44144291:>chr11_KI270721v1_random  AC:KI270721.1  gi:568335394  LN:100316  rg:chr11  rl:unlocalized  M5:9654b5d3f36845bb9d19a6dbd15d2f22  AS:GRCh38
44145726:>chr14_GL000009v2_random  AC:GL000009.2  gi:568335393  LN:201709  rg:chr14  rl:unlocalized  M5:862f555045546733591ff7ab15bcecbe  AS:GRCh38
44148609:>chr14_GL000225v1_random  AC:GL000225.1  gi:224183274  LN:211173  rg:chr14  rl:unlocalized  M5:63945c3e6962f28ffd469719a747e73c  AS:GRCh38
44151627:>chr14_KI270722v1_random  AC:KI270722.1  gi:568335392  LN:194050  rg:chr14  rl:unlocalized  M5:51f46c9093929e6edc3b4dfd50d803fc  AS:GRCh38
44154401:>chr14_GL000194v1_random  AC:GL000194.1  gi:224183213  LN:191469  rg:chr14  rl:unlocalized  M5:6ac8f815bf8e845bb3031b73f812c012  AS:GRCh38
44157138:>chr14_KI270723v1_random  AC:KI270723.1  gi:568335391  LN:38115  rg:chr14  rl:unlocalized  M5:74a4b480675592095fb0c577c515b5df  AS:GRCh38
44157684:>chr14_KI270724v1_random  AC:KI270724.1  gi:568335390  LN:39555  rg:chr14  rl:unlocalized  M5:c3fcb15dddf45f91ef7d94e2623ce13b  AS:GRCh38
44158251:>chr14_KI270725v1_random  AC:KI270725.1  gi:568335389  LN:172810  rg:chr14  rl:unlocalized  M5:edc6402e58396b90b8738a5e37bf773d  AS:GRCh38
44160721:>chr14_KI270726v1_random  AC:KI270726.1  gi:568335388  LN:43739  rg:chr14  rl:unlocalized  M5:fbe54a3197e2b469ccb2f4b161cfbe86  AS:GRCh38
44161347:>chr15_KI270727v1_random  AC:KI270727.1  gi:568335387  LN:448248  rg:chr15  rl:unlocalized  M5:84fe18a7bf03f3b7fc76cbac8eb583f1  AS:GRCh38
44167752:>chr16_KI270728v1_random  AC:KI270728.1  gi:568335386  LN:1872759  rg:chr16  rl:unlocalized  M5:369ff74cf36683b3066a2ca929d9c40d  AS:GRCh38
44194507:>chr17_GL000205v2_random  AC:GL000205.2  gi:568335385  LN:185591  rg:chr17  rl:unlocalized  M5:458e71cd53dd1df4083dc7983a6c82c4  AS:GRCh38
44197160:>chr17_KI270729v1_random  AC:KI270729.1  gi:568335384  LN:280839  rg:chr17  rl:unlocalized  M5:2756f6ee4f5780acce31e995443508b6  AS:GRCh38
44201173:>chr17_KI270730v1_random  AC:KI270730.1  gi:568335383  LN:112551  rg:chr17  rl:unlocalized  M5:48f98ede8e28a06d241ab2e946c15e07  AS:GRCh38
44202782:>chr22_KI270731v1_random  AC:KI270731.1  gi:568335382  LN:150754  rg:chr22  rl:unlocalized  M5:8176d9a20401e8d9f01b7ca8b51d9c08  AS:GRCh38
44204937:>chr22_KI270732v1_random  AC:KI270732.1  gi:568335381  LN:41543  rg:chr22  rl:unlocalized  M5:d837bab5e416450df6e1038ae6cd0817  AS:GRCh38
44205532:>chr22_KI270733v1_random  AC:KI270733.1  gi:568335380  LN:179772  rg:chr22  rl:unlocalized  M5:f1fa05d48bb0c1f87237a28b66f0be0b  AS:GRCh38
44208102:>chr22_KI270734v1_random  AC:KI270734.1  gi:568335379  LN:165050  rg:chr22  rl:unlocalized  M5:1d17410ae2569c758e6dd51616412d32  AS:GRCh38
44210461:>chr22_KI270735v1_random  AC:KI270735.1  gi:568335378  LN:42811  rg:chr22  rl:unlocalized  M5:eb6b07b73dd9a47252098ed3d9fb78b8  AS:GRCh38
44211074:>chr22_KI270736v1_random  AC:KI270736.1  gi:568335377  LN:181920  rg:chr22  rl:unlocalized  M5:2ff189f33cfa52f321accddf648c5616  AS:GRCh38
44213674:>chr22_KI270737v1_random  AC:KI270737.1  gi:568335376  LN:103838  rg:chr22  rl:unlocalized  M5:2ea8bc113a8193d1d700b584b2c5f42a  AS:GRCh38
44215159:>chr22_KI270738v1_random  AC:KI270738.1  gi:568335375  LN:99375  rg:chr22  rl:unlocalized  M5:854ec525c7b6a79e7268f515b6a9877c  AS:GRCh38
44216580:>chr22_KI270739v1_random  AC:KI270739.1  gi:568335374  LN:73985  rg:chr22  rl:unlocalized  M5:760fbd73515fedcc9f37737c4a722d6a  AS:GRCh38
44217638:>chrY_KI270740v1_random  AC:KI270740.1  gi:568335373  LN:37240  rg:chrY  rl:unlocalized  M5:69e42252aead509bf56f1ea6fda91405  AS:GRCh38
44218171:>chrUn_KI270302v1  AC:KI270302.1  gi:568335372  LN:2274  rl:unplaced  M5:ee6dff38036f7d03478c70717643196e  AS:GRCh38
44218205:>chrUn_KI270304v1  AC:KI270304.1  gi:568335371  LN:2165  rl:unplaced  M5:9423c1b46a48aa6331a77ab5c702ac9d  AS:GRCh38
44218237:>chrUn_KI270303v1  AC:KI270303.1  gi:568335370  LN:1942  rl:unplaced  M5:2cb746c78e0faa11e628603a4bc9bd58  AS:GRCh38
44218266:>chrUn_KI270305v1  AC:KI270305.1  gi:568335369  LN:1472  rl:unplaced  M5:f9acea3395b6992cf3418b6689b98cf9  AS:GRCh38
44218289:>chrUn_KI270322v1  AC:KI270322.1  gi:568335368  LN:21476  rl:unplaced  M5:7d459255d1c54369e3b64e719061a5a5  AS:GRCh38
44218597:>chrUn_KI270320v1  AC:KI270320.1  gi:568335367  LN:4416  rl:unplaced  M5:d898b9c5a0118e76481bf5695272959e  AS:GRCh38
44218662:>chrUn_KI270310v1  AC:KI270310.1  gi:568335366  LN:1201  rl:unplaced  M5:af6cb123af7007793bac06485a2a20e9  AS:GRCh38
44218681:>chrUn_KI270316v1  AC:KI270316.1  gi:568335365  LN:1444  rl:unplaced  M5:6adde7a9fe7bd6918f12d0f0924aa8ba  AS:GRCh38
44218703:>chrUn_KI270315v1  AC:KI270315.1  gi:568335364  LN:2276  rl:unplaced  M5:ecc43e822dc011fae1fcfd9981c46e9c  AS:GRCh38
44218737:>chrUn_KI270312v1  AC:KI270312.1  gi:568335363  LN:998  rl:unplaced  M5:26499f2fe4c65621fd8f4ecafbad31d7  AS:GRCh38
44218753:>chrUn_KI270311v1  AC:KI270311.1  gi:568335362  LN:12399  rl:unplaced  M5:59594f9012d8ce21ed5d1119c051a2ba  AS:GRCh38
44218932:>chrUn_KI270317v1  AC:KI270317.1  gi:568335361  LN:37690  rl:unplaced  M5:cd4b1fda800f6ec9ea8001994dbf6499  AS:GRCh38
44219472:>chrUn_KI270412v1  AC:KI270412.1  gi:568335360  LN:1179  rl:unplaced  M5:7bb9612f733fb7f098be79499d46350c  AS:GRCh38
44219490:>chrUn_KI270411v1  AC:KI270411.1  gi:568335359  LN:2646  rl:unplaced  M5:fc240322d91d43c04f349cc58fda3eca  AS:GRCh38
44219529:>chrUn_KI270414v1  AC:KI270414.1  gi:568335358  LN:2489  rl:unplaced  M5:753e02ef3b1c590e0e3376ad2ebb5836  AS:GRCh38
44219566:>chrUn_KI270419v1  AC:KI270419.1  gi:568335357  LN:1029  rl:unplaced  M5:58455e7a788f0dc82034d1fb109f6f5c  AS:GRCh38
44219582:>chrUn_KI270418v1  AC:KI270418.1  gi:568335356  LN:2145  rl:unplaced  M5:1537ec12b9c58d137a2d4cb9db896bbc  AS:GRCh38
44219614:>chrUn_KI270420v1  AC:KI270420.1  gi:568335355  LN:2321  rl:unplaced  M5:bac954a897539c91982a7e3985a49910  AS:GRCh38
44219649:>chrUn_KI270424v1  AC:KI270424.1  gi:568335354  LN:2140  rl:unplaced  M5:747c8f94f34d5de4ad289bc604708210  AS:GRCh38
44219681:>chrUn_KI270417v1  AC:KI270417.1  gi:568335353  LN:2043  rl:unplaced  M5:cd26758fda713f9c96e51d541f50c2d0  AS:GRCh38
44219712:>chrUn_KI270422v1  AC:KI270422.1  gi:568335352  LN:1445  rl:unplaced  M5:3fce80eb4c0554376b591699031feb56  AS:GRCh38
44219734:>chrUn_KI270423v1  AC:KI270423.1  gi:568335351  LN:981  rl:unplaced  M5:bdf5a85c001731dccfb150db2bfe58ac  AS:GRCh38
44219750:>chrUn_KI270425v1  AC:KI270425.1  gi:568335350  LN:1884  rl:unplaced  M5:665a46879bbb48294b0cfa87b61e71f6  AS:GRCh38
44219778:>chrUn_KI270429v1  AC:KI270429.1  gi:568335349  LN:1361  rl:unplaced  M5:ee8962dbef9396884f649e566b78bf06  AS:GRCh38
44219799:>chrUn_KI270442v1  AC:KI270442.1  gi:568335348  LN:392061  rl:unplaced  M5:796289c4cda40e358991f9e672490015  AS:GRCh38
44225401:>chrUn_KI270466v1  AC:KI270466.1  gi:568335347  LN:1233  rl:unplaced  M5:530b7033716a5d72dd544213c513fd12  AS:GRCh38
44225420:>chrUn_KI270465v1  AC:KI270465.1  gi:568335346  LN:1774  rl:unplaced  M5:bb1b2323414425c46531b3c3d22ae00d  AS:GRCh38
44225447:>chrUn_KI270467v1  AC:KI270467.1  gi:568335345  LN:3920  rl:unplaced  M5:db34e0dc109a4afd499b5ec6aaae9754  AS:GRCh38
44225504:>chrUn_KI270435v1  AC:KI270435.1  gi:568335344  LN:92983  rl:unplaced  M5:1655c75415b9c29e143a815f44286d05  AS:GRCh38
44226834:>chrUn_KI270438v1  AC:KI270438.1  gi:568335343  LN:112505  rl:unplaced  M5:e765271939b854dd6826aa764e930c87  AS:GRCh38
44228443:>chrUn_KI270468v1  AC:KI270468.1  gi:568335342  LN:4055  rl:unplaced  M5:0a603090f06108ed7aff75df0767b822  AS:GRCh38
44228502:>chrUn_KI270510v1  AC:KI270510.1  gi:568335341  LN:2415  rl:unplaced  M5:cd7348b3b5d9d0dfef6aed2af75ce920  AS:GRCh38
44228538:>chrUn_KI270509v1  AC:KI270509.1  gi:568335340  LN:2318  rl:unplaced  M5:1cdeb8c823d839e1d1735b5bc9a14856  AS:GRCh38
44228573:>chrUn_KI270518v1  AC:KI270518.1  gi:568335339  LN:2186  rl:unplaced  M5:3fd898b62ca859f50fb8b83e7706352b  AS:GRCh38
44228606:>chrUn_KI270508v1  AC:KI270508.1  gi:568335338  LN:1951  rl:unplaced  M5:7d42a358d472b9cbdfdf30c8742473d0  AS:GRCh38
44228635:>chrUn_KI270516v1  AC:KI270516.1  gi:568335337  LN:1300  rl:unplaced  M5:1cbaaafbbf016906a5bf5886f5a0ecb7  AS:GRCh38
44228655:>chrUn_KI270512v1  AC:KI270512.1  gi:568335336  LN:22689  rl:unplaced  M5:ba1021c82d1230af856f59079e2f71b4  AS:GRCh38
44228981:>chrUn_KI270519v1  AC:KI270519.1  gi:568335335  LN:138126  rl:unplaced  M5:8d754e9c9afd904fba0a2cd577fcc9a1  AS:GRCh38
44230956:>chrUn_KI270522v1  AC:KI270522.1  gi:568335334  LN:5674  rl:unplaced  M5:070b4678e22501029c2e3297115216bc  AS:GRCh38
44231039:>chrUn_KI270511v1  AC:KI270511.1  gi:568335333  LN:8127  rl:unplaced  M5:907ca34a4a2a6673632ebaf513a4c1a4  AS:GRCh38
44231157:>chrUn_KI270515v1  AC:KI270515.1  gi:568335332  LN:6361  rl:unplaced  M5:dd7527ee8e0bdb0a43ca0b2a5456c8c3  AS:GRCh38
44231249:>chrUn_KI270507v1  AC:KI270507.1  gi:568335331  LN:5353  rl:unplaced  M5:311894d0a815eb07c5cac49da851cb4a  AS:GRCh38
44231327:>chrUn_KI270517v1  AC:KI270517.1  gi:568335330  LN:3253  rl:unplaced  M5:913440c38d95c618617ca69bb9296170  AS:GRCh38
44231375:>chrUn_KI270529v1  AC:KI270529.1  gi:568335329  LN:1899  rl:unplaced  M5:4caf890f2586daab8e4b2e2db904f05f  AS:GRCh38
44231404:>chrUn_KI270528v1  AC:KI270528.1  gi:568335328  LN:2983  rl:unplaced  M5:d75c9235f0b8c449fc4352997c56b086  AS:GRCh38
44231448:>chrUn_KI270530v1  AC:KI270530.1  gi:568335327  LN:2168  rl:unplaced  M5:04549369e1197c626669a10164613635  AS:GRCh38
44231480:>chrUn_KI270539v1  AC:KI270539.1  gi:568335326  LN:993  rl:unplaced  M5:19e3a982e67eafef39c5a3e4163f1e17  AS:GRCh38
44231496:>chrUn_KI270538v1  AC:KI270538.1  gi:568335325  LN:91309  rl:unplaced  M5:d60b72221cc7af871f2c757577e4c92a  AS:GRCh38
44232802:>chrUn_KI270544v1  AC:KI270544.1  gi:568335324  LN:1202  rl:unplaced  M5:e62a14b14467cdf48b5a236c66918f0f  AS:GRCh38
44232821:>chrUn_KI270548v1  AC:KI270548.1  gi:568335323  LN:1599  rl:unplaced  M5:866b0db8e9cf66208c2c064bd09ce0a2  AS:GRCh38
44232845:>chrUn_KI270583v1  AC:KI270583.1  gi:568335322  LN:1400  rl:unplaced  M5:b127e2e6dbe358ff192b271b8c6ee690  AS:GRCh38
44232866:>chrUn_KI270587v1  AC:KI270587.1  gi:568335321  LN:2969  rl:unplaced  M5:36be47659719f47b95caa51744aa8f70  AS:GRCh38
44232910:>chrUn_KI270580v1  AC:KI270580.1  gi:568335320  LN:1553  rl:unplaced  M5:1df30dae0f605811d927dcea58e729fc  AS:GRCh38
44232934:>chrUn_KI270581v1  AC:KI270581.1  gi:568335319  LN:7046  rl:unplaced  M5:9f26945f9f9b3f865c9ebe953cbbc1a9  AS:GRCh38
44233036:>chrUn_KI270579v1  AC:KI270579.1  gi:568335318  LN:31033  rl:unplaced  M5:fe62fb1964002717cc1b034630e89b1f  AS:GRCh38
44233481:>chrUn_KI270589v1  AC:KI270589.1  gi:568335317  LN:44474  rl:unplaced  M5:211c215414693fe0a2399cf82e707e03  AS:GRCh38
44234118:>chrUn_KI270590v1  AC:KI270590.1  gi:568335316  LN:4685  rl:unplaced  M5:e8a57f147561b361091791b9010cd28b  AS:GRCh38
44234186:>chrUn_KI270584v1  AC:KI270584.1  gi:568335315  LN:4513  rl:unplaced  M5:d93636c9d54abd013cfc0d4c01334032  AS:GRCh38
44234252:>chrUn_KI270582v1  AC:KI270582.1  gi:568335314  LN:6504  rl:unplaced  M5:6fd9804a7478d2e28160fe9f017689cb  AS:GRCh38
44234346:>chrUn_KI270588v1  AC:KI270588.1  gi:568335313  LN:6158  rl:unplaced  M5:37ffa850e69b342a8f8979bd3ffc77d4  AS:GRCh38
44234435:>chrUn_KI270593v1  AC:KI270593.1  gi:568335312  LN:3041  rl:unplaced  M5:f4a5bfa203e9e81acb640b18fb11e78e  AS:GRCh38
44234480:>chrUn_KI270591v1  AC:KI270591.1  gi:568335311  LN:5796  rl:unplaced  M5:d6af509d69835c9ac25a30086e5a4051  AS:GRCh38
44234564:>chrUn_KI270330v1  AC:KI270330.1  gi:568335310  LN:1652  rl:unplaced  M5:c2c590706a339007b00c59e0b8937e78  AS:GRCh38
44234589:>chrUn_KI270329v1  AC:KI270329.1  gi:568335309  LN:1040  rl:unplaced  M5:f023f927ae84c5cc48dc4dce11ba90f2  AS:GRCh38
44234605:>chrUn_KI270334v1  AC:KI270334.1  gi:568335308  LN:1368  rl:unplaced  M5:53afe12d1371f250a3d1de655345d374  AS:GRCh38
44234626:>chrUn_KI270333v1  AC:KI270333.1  gi:568335307  LN:2699  rl:unplaced  M5:57baf650c47bba9b3a8b7c6d0fb55ad6  AS:GRCh38
44234666:>chrUn_KI270335v1  AC:KI270335.1  gi:568335306  LN:1048  rl:unplaced  M5:eb27188639503b524d2659a23b8262ea  AS:GRCh38
44234682:>chrUn_KI270338v1  AC:KI270338.1  gi:568335305  LN:1428  rl:unplaced  M5:301ef75a6b2996d745eb3464bd352b57  AS:GRCh38
44234704:>chrUn_KI270340v1  AC:KI270340.1  gi:568335304  LN:1428  rl:unplaced  M5:56b462bac20d385cdfcde0155fe4c3a1  AS:GRCh38
44234726:>chrUn_KI270336v1  AC:KI270336.1  gi:568335303  LN:1026  rl:unplaced  M5:69ad2d85d870c8b0269434581e86e30e  AS:GRCh38
44234742:>chrUn_KI270337v1  AC:KI270337.1  gi:568335302  LN:1121  rl:unplaced  M5:16fc8d71a2662a6cfec7bdeec3d810c6  AS:GRCh38
44234760:>chrUn_KI270363v1  AC:KI270363.1  gi:568335301  LN:1803  rl:unplaced  M5:6edd17a912f391022edbc192d49f2489  AS:GRCh38
44234787:>chrUn_KI270364v1  AC:KI270364.1  gi:568335300  LN:2855  rl:unplaced  M5:6ff66a8e589ca27d93b5bac0e5b13a87  AS:GRCh38
44234829:>chrUn_KI270362v1  AC:KI270362.1  gi:568335299  LN:3530  rl:unplaced  M5:bc82401ffd9a5ae711fa0ea34da8d2f0  AS:GRCh38
44234881:>chrUn_KI270366v1  AC:KI270366.1  gi:568335298  LN:8320  rl:unplaced  M5:44a0b65b7ba6bcff37eca202e7d966ea  AS:GRCh38
44235001:>chrUn_KI270378v1  AC:KI270378.1  gi:568335297  LN:1048  rl:unplaced  M5:fc13bda7dbd914c92fb7e49489d1350f  AS:GRCh38
44235017:>chrUn_KI270379v1  AC:KI270379.1  gi:568335296  LN:1045  rl:unplaced  M5:3218bef25946cd95de585dfc7750f63b  AS:GRCh38
44235033:>chrUn_KI270389v1  AC:KI270389.1  gi:568335295  LN:1298  rl:unplaced  M5:2c9b08c57c27e714d4d5259fd91b6983  AS:GRCh38
44235053:>chrUn_KI270390v1  AC:KI270390.1  gi:568335294  LN:2387  rl:unplaced  M5:7a64d89ea14990c16d20f4d6e7283e10  AS:GRCh38
44235089:>chrUn_KI270387v1  AC:KI270387.1  gi:568335293  LN:1537  rl:unplaced  M5:22a12462264340c25e912b8485cdfa91  AS:GRCh38
44235112:>chrUn_KI270395v1  AC:KI270395.1  gi:568335292  LN:1143  rl:unplaced  M5:7c03ca4756c1620f318fb189214388d8  AS:GRCh38
44235130:>chrUn_KI270396v1  AC:KI270396.1  gi:568335291  LN:1880  rl:unplaced  M5:9069bed3c2efe7cc87227d619ad5816f  AS:GRCh38
44235158:>chrUn_KI270388v1  AC:KI270388.1  gi:568335290  LN:1216  rl:unplaced  M5:76f9f3315fa4b831e93c36cd88196480  AS:GRCh38
44235177:>chrUn_KI270394v1  AC:KI270394.1  gi:568335289  LN:970  rl:unplaced  M5:d5171e863a3d8f832f0559235987b1e5  AS:GRCh38
44235192:>chrUn_KI270386v1  AC:KI270386.1  gi:568335288  LN:1788  rl:unplaced  M5:b9b1baaa7abf206f6b70cf31654172db  AS:GRCh38
44235219:>chrUn_KI270391v1  AC:KI270391.1  gi:568335287  LN:1484  rl:unplaced  M5:1fa5cf03b3eac0f1b4a64948fd09de53  AS:GRCh38
44235242:>chrUn_KI270383v1  AC:KI270383.1  gi:568335286  LN:1750  rl:unplaced  M5:694d75683e4a9554bcc1291edbcaee43  AS:GRCh38
44235268:>chrUn_KI270393v1  AC:KI270393.1  gi:568335285  LN:1308  rl:unplaced  M5:3724e1d70677d6b5c4bcf17fd40da111  AS:GRCh38
44235288:>chrUn_KI270384v1  AC:KI270384.1  gi:568335284  LN:1658  rl:unplaced  M5:b06e44ea15d0a57618d6ca7d2e6ac5d2  AS:GRCh38
44235313:>chrUn_KI270392v1  AC:KI270392.1  gi:568335283  LN:971  rl:unplaced  M5:59b3ca8de65fb171683f8a06d3b4bf0d  AS:GRCh38
44235328:>chrUn_KI270381v1  AC:KI270381.1  gi:568335282  LN:1930  rl:unplaced  M5:2a9297cfd3b3807195ab9ad07e775d99  AS:GRCh38
44235357:>chrUn_KI270385v1  AC:KI270385.1  gi:568335281  LN:990  rl:unplaced  M5:112a8b1df94ef0498a0bfe2d2ea5cc23  AS:GRCh38
44235373:>chrUn_KI270382v1  AC:KI270382.1  gi:568335280  LN:4215  rl:unplaced  M5:e7085cdcee6ad62f359744e13d3209fc  AS:GRCh38
44235435:>chrUn_KI270376v1  AC:KI270376.1  gi:568335279  LN:1136  rl:unplaced  M5:59e8fc80b78d62325082334b43dffdba  AS:GRCh38
44235453:>chrUn_KI270374v1  AC:KI270374.1  gi:568335278  LN:2656  rl:unplaced  M5:dbc92c9a92e558946e58b4909ec95dd5  AS:GRCh38
44235492:>chrUn_KI270372v1  AC:KI270372.1  gi:568335277  LN:1650  rl:unplaced  M5:53a9d5e8fd28bce5da5efcfd9114dbf2  AS:GRCh38
44235517:>chrUn_KI270373v1  AC:KI270373.1  gi:568335276  LN:1451  rl:unplaced  M5:b174fe53be245a840cd6324e39b88ced  AS:GRCh38
44235539:>chrUn_KI270375v1  AC:KI270375.1  gi:568335275  LN:2378  rl:unplaced  M5:d678250c97e9b94aa390fa46e70a6d83  AS:GRCh38
44235574:>chrUn_KI270371v1  AC:KI270371.1  gi:568335274  LN:2805  rl:unplaced  M5:a0af3d778dfeb7963e8e6d84c0c54fba  AS:GRCh38
44235616:>chrUn_KI270448v1  AC:KI270448.1  gi:568335273  LN:7992  rl:unplaced  M5:0f40827c265cb813b6e723da6c9b926b  AS:GRCh38
44235732:>chrUn_KI270521v1  AC:KI270521.1  gi:568335272  LN:7642  rl:unplaced  M5:af5bef7cefec7bd7efa729ac6c5be088  AS:GRCh38
44235843:>chrUn_GL000195v1  AC:GL000195.1  gi:224183229  LN:182896  rl:unplaced  M5:5d9ec007868d517e73543b005ba48535  AS:GRCh38
44238457:>chrUn_GL000219v1  AC:GL000219.1  gi:224183268  LN:179198  rl:unplaced  M5:f977edd13bac459cb2ed4a5457dba1b3  AS:GRCh38
44241018:>chrUn_GL000220v1  AC:GL000220.1  gi:224183269  LN:161802  rl:unplaced  M5:fc35de963c57bf7648429e6454f1c9db  AS:GRCh38
44243331:>chrUn_GL000224v1  AC:GL000224.1  gi:224183273  LN:179693  rl:unplaced  M5:d5b2fc04f6b41b212a4198a07f450e20  AS:GRCh38
44245900:>chrUn_KI270741v1  AC:KI270741.1  gi:568335271  LN:157432  rl:unplaced  M5:86eaea8a15a3950e37442eaaa5c9dc92  AS:GRCh38
44248151:>chrUn_GL000226v1  AC:GL000226.1  gi:224183275  LN:15008  rl:unplaced  M5:1c1b2cd1fccbc0a99b6a447fa24d1504  AS:GRCh38
44248367:>chrUn_GL000213v1  AC:GL000213.1  gi:224183287  LN:164239  rl:unplaced  M5:9d424fdcc98866650b58f004080a992a  AS:GRCh38
44250715:>chrUn_KI270743v1  AC:KI270743.1  gi:568335270  LN:210658  rl:unplaced  M5:3b62d9d3100f530d509e4efebd98502c  AS:GRCh38
44253726:>chrUn_KI270744v1  AC:KI270744.1  gi:568335269  LN:168472  rl:unplaced  M5:e90aee46b947ff8c32291a6843fde3f9  AS:GRCh38
44256134:>chrUn_KI270745v1  AC:KI270745.1  gi:568335268  LN:41891  rl:unplaced  M5:1386fe3de6f82956f2124e19353ff9c1  AS:GRCh38
44256734:>chrUn_KI270746v1  AC:KI270746.1  gi:568335267  LN:66486  rl:unplaced  M5:c470486a0a858e14aa21d7866f83cc17  AS:GRCh38
44257685:>chrUn_KI270747v1  AC:KI270747.1  gi:568335266  LN:198735  rl:unplaced  M5:62375d812ece679c9fd2f3d08d4e22a4  AS:GRCh38
44260526:>chrUn_KI270748v1  AC:KI270748.1  gi:568335265  LN:93321  rl:unplaced  M5:4f6c6ab005c852a4352aa33e7cc88ded  AS:GRCh38
44261861:>chrUn_KI270749v1  AC:KI270749.1  gi:568335264  LN:158759  rl:unplaced  M5:c899a7b4e911d371283f3f4058ca08b7  AS:GRCh38
44264130:>chrUn_KI270750v1  AC:KI270750.1  gi:568335263  LN:148850  rl:unplaced  M5:c022ba92f244b7dc54ea90c4eef4d554  AS:GRCh38
44266258:>chrUn_KI270751v1  AC:KI270751.1  gi:568335262  LN:150742  rl:unplaced  M5:1b758bbdee0e9ca882058d916cba9d29  AS:GRCh38
44268413:>chrUn_KI270752v1  AC:KI270752.1  gi:568335261  LN:27745  rl:unplaced  M5:e0880631848337bd58559d9b1519da63  AS:GRCh38
44268811:>chrUn_KI270753v1  AC:KI270753.1  gi:568335260  LN:62944  rl:unplaced  M5:25075fb2a1ecada67c0eb2f1fe0c7ec9  AS:GRCh38
44269712:>chrUn_KI270754v1  AC:KI270754.1  gi:568335259  LN:40191  rl:unplaced  M5:fe9e16233cecbc244f06f3acff3d03b8  AS:GRCh38
44270288:>chrUn_KI270755v1  AC:KI270755.1  gi:568335258  LN:36723  rl:unplaced  M5:4a7da6a658955bd787af8add3ccb5751  AS:GRCh38
44270814:>chrUn_KI270756v1  AC:KI270756.1  gi:568335257  LN:79590  rl:unplaced  M5:2996b120a5a5e15dab6555f0bf92e374  AS:GRCh38
44271952:>chrUn_KI270757v1  AC:KI270757.1  gi:568335256  LN:71251  rl:unplaced  M5:174c73b60b41d8a1ef0fbaa4b3bdf0d3  AS:GRCh38
44272971:>chrUn_GL000214v1  AC:GL000214.1  gi:224183298  LN:137718  rl:unplaced  M5:46c2032c37f2ed899eb41c0473319a69  AS:GRCh38
44274940:>chrUn_KI270742v1  AC:KI270742.1  gi:568335255  LN:186739  rl:unplaced  M5:2f31c013a4a8301deb8ab7ed1ca1cd99  AS:GRCh38
44277609:>chrUn_GL000216v2  AC:GL000216.2  gi:568335254  LN:176608  rl:unplaced  M5:725009a7e3f5b78752b68afa922c090c  AS:GRCh38
44280133:>chrUn_GL000218v1  AC:GL000218.1  gi:224183305  LN:161147  rl:unplaced  M5:1d708b54644c26c7e01c2dad5426d38c  AS:GRCh38
44282437:>chrEBV  AC:AJ507799.2  gi:86261677  LN:171823  rl:decoy  M5:6743bd63b3ff2b5b8985d8933c53290a  SP:Human_herpesvirus_4  tp:circular
```

Make a copy of `genome.fa`. Remove the [Alternate Loci](https://www.ncbi.nlm.nih.gov/grc/help/definitions/).
```
$ mkdir -p NCBI_GRCh38_22XYM_order/Sequence/Bowtie2Index NCBI_GRCh38_22XYM_order/Sequence/WholeGenomeFasta
$ cd NCBI_GRCh38_22XYM_order/Sequence/WholeGenomeFasta/
$ cp ../../../NCBI_GRCh38_22XYM_AL_order/Sequence/WholeGenomeFasta/genome.fa .
$ sed -i '44118414,$d' genome.fa

$ grep '^>' genome.fa -n
1:>chr1  AC:CM000663.2  gi:568336023  LN:248956422  rl:Chromosome  M5:6aef897c3d6ff0c78aff06ac189178dd  AS:GRCh38
3556523:>chr2  AC:CM000664.2  gi:568336022  LN:242193529  rl:Chromosome  M5:f98db672eb0993dcfdabafe2a882905c  AS:GRCh38
7016432:>chr3  AC:CM000665.2  gi:568336021  LN:198295559  rl:Chromosome  M5:76635a41ea913a405ded820447d067b0  AS:GRCh38
9849227:>chr4  AC:CM000666.2  gi:568336020  LN:190214555  rl:Chromosome  M5:3210fecf1eb92d5489da4346b3fddc6e  AS:GRCh38
12566579:>chr5  AC:CM000667.2  gi:568336019  LN:181538259  rl:Chromosome  M5:a811b3dc9fe66af729dc0dddf7fa4f13  AS:GRCh38  hm:47309185-49591369
15159984:>chr6  AC:CM000668.2  gi:568336018  LN:170805979  rl:Chromosome  M5:5691468a67c7e7a7b5f2a3a683792c29  AS:GRCh38
17600071:>chr7  AC:CM000669.2  gi:568336017  LN:159345973  rl:Chromosome  M5:cc044cc2256a1141212660fb07b6171e  AS:GRCh38
19876444:>chr8  AC:CM000670.2  gi:568336016  LN:145138636  rl:Chromosome  M5:c67955b5f7815a9a1edfaa15893d3616  AS:GRCh38
21949855:>chr9  AC:CM000671.2  gi:568336015  LN:138394717  rl:Chromosome  M5:6c198acf68b5af7b9d676dfdd531b5de  AS:GRCh38
23926924:>chr10  AC:CM000672.2  gi:568336014  LN:133797422  rl:Chromosome  M5:c0eeee7acfdaf31b770a509bdaa6e51a  AS:GRCh38
25838317:>chr11  AC:CM000673.2  gi:568336013  LN:135086622  rl:Chromosome  M5:1511375dc2dd1b633af8cf439ae90cec  AS:GRCh38
27768127:>chr12  AC:CM000674.2  gi:568336012  LN:133275309  rl:Chromosome  M5:96e414eace405d8c27a6d35ba19df56f  AS:GRCh38
29672061:>chr13  AC:CM000675.2  gi:568336011  LN:114364328  rl:Chromosome  M5:a5437debe2ef9c9ef8f3ea2874ae1d82  AS:GRCh38
31305839:>chr14  AC:CM000676.2  gi:568336010  LN:107043718  rl:Chromosome  M5:e0f0eecc3bcab6178c62b6211565c807  AS:GRCh38  hm:multiple
32835036:>chr15  AC:CM000677.2  gi:568336009  LN:101991189  rl:Chromosome  M5:f036bd11158407596ca6bf3581454706  AS:GRCh38
34292054:>chr16  AC:CM000678.2  gi:568336008  LN:90338345  rl:Chromosome  M5:db2d37c8b7d019caaf2dd64ba3a6f33a  AS:GRCh38
35582603:>chr17  AC:CM000679.2  gi:568336007  LN:83257441  rl:Chromosome  M5:f9a0fb01553adb183568e3eb9d8626db  AS:GRCh38
36771997:>chr18  AC:CM000680.2  gi:568336006  LN:80373285  rl:Chromosome  M5:11eeaa801f6b0e2e36a1138616b8ee9a  AS:GRCh38
37920188:>chr19  AC:CM000681.2  gi:568336005  LN:58617616  rl:Chromosome  M5:85f9f4fc152c58cb7913c06d6b98573a  AS:GRCh38  hm:multiple
38757584:>chr20  AC:CM000682.2  gi:568336004  LN:64444167  rl:Chromosome  M5:b18e6c531b0bd70e949a7fc20859cb01  AS:GRCh38
39678216:>chr21  AC:CM000683.2  gi:568336003  LN:46709983  rl:Chromosome  M5:974dc7aec0b755b19f031418fdedf293  AS:GRCh38  hm:multiple
40345503:>chr22  AC:CM000684.2  gi:568336002  LN:50818468  rl:Chromosome  M5:ac37ec46683600f808cdd41eac1d55cd  AS:GRCh38  hm:multiple
41071483:>chrX  AC:CM000685.2  gi:568336001  LN:156040895  rl:Chromosome  M5:2b3a55ff7f58eb308420c8a9b11cac50  AS:GRCh38
43300640:>chrY  AC:CM000686.2  gi:568336000  LN:57227415  rl:Chromosome  M5:ce3e31103314a704255f3cd90369ecce  AS:GRCh38  hm:10001-2781479,56887903-57217415
44118176:>chrM  AC:J01415.2  gi:113200490  LN:16569  rl:Mitochondrion  M5:c68f52674c9fb33aef52dcf399755519  AS:GRCh38  tp:circular
```

Build Bowtie2 index for it.
```bash
$ nohup bowtie2-build genome.fa ../Bowtie2Index/genome &
```



Cleaned the directory struncture to remove redundant file folders. Final file tree.
```bash
$ tree -L 3
.
└── ref
    └── hg
        ├── hg19_22XYM_order
        ├── hg19_M22XY_order
        ├── hg38_22XYM_AL_order
        ├── hg38_22XYM_order
        ├── NCBI_build37.2
        ├── NCBI_GRCh38_22XYM_AL_order
        └── NCBI_GRCh38_22XYM_order
```
Abbreviation representation:

22 for chromosome(chr) 1 - chr 22, X for chrX, Y for chrY, M for Mitochondrial chromosome. AL for [Alternate Loci](https://www.ncbi.nlm.nih.gov/grc/help/definitions/).

```bash
$ tree -L 5
.
└── ref
    └── hg
        ├── hg19_22XYM_order
        │   └── Sequence
        │       ├── Bowtie2Index
        │       └── WholeGenomeFasta
        ├── hg19_M22XY_order
        │   ├── Annotation
        │   │   ├── Archives
        │   │   ├── Genes -> Archives/archive-current/Genes
        │   │   ├── README.txt -> Archives/archive-current/README.txt
        │   │   ├── SmallRNA -> Archives/archive-current/SmallRNA
        │   │   └── Variation -> Archives/archive-current/Variation
        │   ├── Homo_sapiens_UCSC_hg19.tar.gz
        │   ├── README.txt
        │   └── Sequence
        │       ├── AbundantSequences
        │       ├── BlastDB
        │       ├── Bowtie2Index
        │       ├── BowtieIndex
        │       ├── BWAIndex
        │       ├── Chromosomes
        │       ├── MDSBowtieIndex
        │       └── WholeGenomeFasta
        ├── hg38_22XYM_AL_order
        │   ├── Annotation
        │   │   ├── Archives
        │   │   ├── Genes -> Archives/archive-2015-08-14-08-18-15/Genes
        │   │   ├── Genes.gencode -> Archives/archive-2015-08-14-08-18-15/Genes.gencode
        │   │   └── SmallRNA -> Archives/archive-2015-08-14-08-18-15/SmallRNA
        │   ├── README.txt
        │   └── Sequence
        │       ├── AbundantSequences
        │       ├── Bowtie2Index
        │       ├── BowtieIndex
        │       ├── BWAIndex
        │       ├── Chromosomes
        │       └── WholeGenomeFasta
        ├── hg38_22XYM_order
        │   └── Sequence
        │       ├── Bowtie2Index
        │       └── WholeGenomeFasta
        ├── NCBI_build37.2
        │   ├── Annotation
        │   │   ├── Archives
        │   │   ├── Genes -> Archives/archive-current/Genes
        │   │   ├── README.txt -> Archives/archive-current/README.txt
        │   │   ├── SmallRNA -> Archives/archive-current/SmallRNA
        │   │   └── Variation -> Archives/archive-current/Variation
        │   ├── README.txt
        │   └── Sequence
        │       ├── AbundantSequences
        │       ├── Bowtie2Index
        │       ├── BowtieIndex
        │       ├── BWAIndex
        │       ├── Chromosomes
        │       ├── Methylation
        │       └── WholeGenomeFasta
        ├── NCBI_GRCh38_22XYM_AL_order
        │   ├── Annotation
        │   │   ├── Archives
        │   │   ├── Genes -> Archives/archive-2015-08-11-09-31-31/Genes
        │   │   ├── Genes.gencode -> Archives/archive-2015-08-11-09-31-31/Genes.gencode
        │   │   └── SmallRNA -> Archives/archive-2015-08-11-09-31-31/SmallRNA
        │   ├── README.txt
        │   └── Sequence
        │       ├── AbundantSequences
        │       ├── Bowtie2Index
        │       ├── BowtieIndex
        │       ├── BWAIndex
        │       ├── Chromosomes
        │       └── WholeGenomeFasta
        └── NCBI_GRCh38_22XYM_order
            └── Sequence
                ├── Bowtie2Index
                └── WholeGenomeFasta
```

`*/Sequence/WholeGenomeFasta/genome.fa` is the genome fasta file. `*/Sequence/Bowtie2Index` contains the bowtie2 index files. BWAIndex for some of them are going to be built.
