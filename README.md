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

# hisat report
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

## Prepare the SAM file for assembly
 ```
# install Samtools

# convert the SAM file into BAM file then sort it

use stringtie to get transcript

## Transcript 

Assembly without known annotations have 1680 transcript 

Assembly with known previous annotations have 1814 transcript 
 ```
        
        
