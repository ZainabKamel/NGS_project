## NGS_project
```
for the code open modified_code file
```
# Data Downloading   
1. Sample Data
```
   Check the QC of the data and GC content from the "multiqc_report.html" file
   ```
2. Transcriptome 
3. Annotation file 

# Alignment
1. Install hisat
2. mkdir ~/project_ngs1/hisat_align/hisat_index && cd ~/project_ngs1/hisat_align/hisat_index
3. gunzip data 
4. Install seqtk 
5. subsetting to 5M by seqtk package
6. Indexing 
7. gzip 

# hisat report for 1ry Alignment
```
5000000 reads; of these:
  5000000 (100.00%) were paired; of these:
    4827233 (96.54%) aligned concordantly 0 times
    159564 (3.19%) aligned concordantly exactly 1 time
    13203 (0.26%) aligned concordantly >1 times
    ---- 
    4827233 pairs aligned concordantly 0 times; of these:
      2462 (0.05%) aligned discordantly 1 time
    ----
    4824771 pairs aligned 0 times concordantly or discordantly; of these:
      9649542 mates make up the pairs; of these:
        9534026 (98.80%) aligned 0 times
        79790 (0.83%) aligned exactly 1 time
        35726 (0.37%) aligned >1 times
4.66% overall alignment rate
 ```
# hisat report for 2ry Alignment
```
5000000 reads; of these:
  5000000 (100.00%) were paired; of these:
    4824102 (96.48%) aligned concordantly 0 times
    116079 (2.32%) aligned concordantly exactly 1 time
    59819 (1.20%) aligned concordantly >1 times
    ----
    4824102 pairs aligned concordantly 0 times; of these:
      1413 (0.03%) aligned discordantly 1 time
    ----
    4822689 pairs aligned 0 times concordantly or discordantly; of these:
      9645378 mates make up the pairs; of these:
        9531120 (98.82%) aligned 0 times
        52847 (0.55%) aligned exactly 1 time
        61411 (0.64%) aligned >1 times
4.69% overall alignment rate
```

## Prepare the SAM & BAM files for assembly
 ```
# install Samtools

# convert the SAM file into BAM file then sort it

use stringtie to get transcript
 ```
## Transcript 
```
1ry Assembly without known annotations have 1680 transcript  

1ry Assembly with known previous annotations have 1814 transcript 

2ry Assembly without known annotations have 1641 transcript  

2ry Assembly with known previous annotations have 1786 transcript
 ```
## Comparing GTF files of 1ry & 2ry Alignment
 ```
# Install gffcompare
```
# Run for ref free Report
 ```
gffcompare v0.11.2 | Command line was:
gffcompare -r /home/ngs/workdir/project_ngs1/hisat_align/ref_free.gtf /home/ngs/workdir/project_ngs1/hisat_align/ref_free_sec.gtf

= Summary for dataset: /home/ngs/workdir/project_ngs1/hisat_align/ref_free_sec.gtf 
     Query mRNAs :    1641 in    1622 loci  (1571 multi-exon transcripts)
            (18 multi-transcript loci, ~1.0 transcripts per locus)
 Reference mRNAs :    1680 in    1653 loci  (1592 multi-exon)
 Super-loci w/ reference transcripts:     1514
-----------------| Sensitivity | Precision  |
        Base level:    89.9     |    92.6    |
        Exon level:    91.5     |    93.6    |
      Intron level:    93.9     |    96.1    |
Intron chain level:    87.2     |    88.4    |
  Transcript level:    85.0     |    87.0    |
       Locus level:    85.3     |    86.9    |

     Matching intron chains:    1388
       Matching transcripts:    1428
              Matching loci:    1410

          Missed exons:     351/5596	(  6.3%)
           Novel exons:     234/5469	(  4.3%)
        Missed introns:     233/3927	(  5.9%)
         Novel introns:     145/3835	(  3.8%)
           Missed loci:     133/1653	(  8.0%)
            Novel loci:      92/1622	(  5.7%)

 Total union super-loci across all input datasets: 1606 
1641 out of 1641 consensus transcripts written in gffcmp.annotated.gtf (0 discarded as redundant)

  ```
# Run for ref sup
 ```
 gffcompare v0.11.2 | Command line was:
gffcompare -r /home/ngs/workdir/project_ngs1/hisat_align/ref_sup.gtf /home/ngs/workdir/project_ngs1/hisat_align/ref_sup_sec.gtf


= Summary for dataset: /home/ngs/workdir/project_ngs1/hisat_align/ref_sup_sec.gtf 
     Query mRNAs :    1786 in    1708 loci  (1672 multi-exon transcripts)
            (59 multi-transcript loci, ~1.0 transcripts per locus)
 Reference mRNAs :    1810 in    1730 loci  (1681 multi-exon)
 Super-loci w/ reference transcripts:     1594
-----------------| Sensitivity | Precision  |
        Base level:    90.7     |    91.7    |
        Exon level:    91.3     |    93.1    |
      Intron level:    93.9     |    95.9    |
Intron chain level:    86.9     |    87.4    |
  Transcript level:    85.5     |    86.7    |
       Locus level:    85.7     |    86.8    |

     Matching intron chains:    1461
       Matching transcripts:    1548
              Matching loci:    1482

          Missed exons:     374/6166	(  6.1%)
           Novel exons:     262/6050	(  4.3%)
        Missed introns:     256/4334	(  5.9%)
         Novel introns:     166/4240	(  3.9%)
           Missed loci:     127/1730	(  7.3%)
            Novel loci:      99/1708	(  5.8%)

 Total union super-loci across all input datasets: 1693 
1786 out of 1786 consensus transcripts written in gffcmp.annotated.gtf (0 discarded as redundant)
 
  ```
        
