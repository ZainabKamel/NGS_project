# NGS_project


#Data Download
1. Sample Data
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra49/SRZ/010956/SRR10956573/MC_MYR2_R1.fastq.gz
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra49/SRZ/010956/SRR10956573/MC_MYR2_R2.fastq.gz

2. Transcriptome 
wget ftp://ftp.ensembl.org/pub/release-99/fasta/danio_rerio/dna/Danio_rerio.GRCz11.dna.toplevel.fa.gz

wget ftp://ftp.ensembl.org/pub/release-99/fasta/danio_rerio/dna/Danio_rerio.GRCz11.dna.chromosome.10.fa.gz

3. Annotation file 
wget ftp://ftp.ensembl.org/pub/release-99/gtf/danio_rerio/Danio_rerio.GRCz11.99.chr.gtf.gz

#Hisat
1. Install hisat
conda install -c bioconda -y hisat2

2. mkdir ~/project_ngs1/hisat_align/hisat_index && cd ~/project_ngs1/hisat_align/hisat_index

3. gunzip data 
4. Install seqtk 
5. subsetting to 5M 
seqtk sample -s100 MC_MYR2_R1.fastq 5000000 > MC_MYR2_R1_5M.fastq

6. Indexing 
hisat2_extract_splice_sites.py ~/project_ngs1/sample_data/Danio_rerio.GRCz11.99.chr.gtf > splicesites.tsv
hisat2_extract_exons.py ~/project_ngs1/sample_data/Danio_rerio.GRCz11.99.chr.gtf > exons.tsv
hisat2-build -p 1 --ss splicesites.tsv --exon exons.tsv Danio_rerio.GRCz11.dna.chromosome.10.fa Danio_rerio.GRCz11


7. gzip 
R1="$HOME/project_ngs1/sample_data/MC_MYR2_R1_5M.fastq.gz"
R2="$HOME/project_ngs1/sample_data/MC_MYR2_R2_5M.fastq.gz"
hisat2 -p 1 -x hisat_index/Danio_rerio.GRCz11 --dta --rna-strandness RF -1 $R1 -2 $R2 -S align.sam
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
