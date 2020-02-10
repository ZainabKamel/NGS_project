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
        
        
