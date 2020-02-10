## create the conda env if you have not already did
```
conda create -y --name project python=3.6
conda activate project

mkdir ~/workdir/project_ngs1/sample_data && cd ~/workdir/project_ngs1/sample_data
```

## Downloading data 
```
# Sample Data 
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra49/SRZ/010956/SRR10956573/MC_MYR2_R1.fastq.gz
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra49/SRZ/010956/SRR10956573/MC_MYR2_R2.fastq.gz
gunzip MC_MYR2_R1.fastq.gz
gunzip MC_MYR2_R2.fastq.gz

# Transcriptome
#wget ftp://ftp.ensembl.org/pub/release-99/fasta/danio_rerio/dna/Danio_rerio.GRCz11.dna.toplevel.fa.gz
wget ftp://ftp.ensembl.org/pub/release-99/fasta/danio_rerio/dna/Danio_rerio.GRCz11.dna.chromosome.10.fa.gz
gunzip Danio_rerio.GRCz11.dna.chromosome.10.fa.gz

#Annotation file 
wget ftp://ftp.ensembl.org/pub/release-99/gtf/danio_rerio/Danio_rerio.GRCz11.99.chr.gtf.gz
gunzip Danio_rerio.GRCz11.99.chr.gtf.gz
```

## to get QC Report
```
# Install the QC file generator
conda install -c bioconda fastqc 
conda install -c bioconda multiqc 

# Run the FASTQC for each read end
for f in ~/workdir/project_ngs1/sample_data/*.fastq.gz;do fastqc -t 1 -f fastq -noextract $f;done

mkdir -p ~/workdir/project_ngs1/FASTQC && cd ~/workdir/project_ngs1/FASTQC
# Merge the output reports into one super report
mv ../sample_data/*html ./
mv ../sample_data/*zip ./
multiqc -z -o . .
```

```
# Download seqtk
sudo apt install seqtk

# subsetting to 5M 
seqtk sample -s100 MC_MYR2_R1.fastq 5000000 > MC_MYR2_R1_5M.fastq
seqtk sample -s100 MC_MYR2_R2.fastq 5000000 > MC_MYR2_R2_5M.fastq
# compress files
gzip MC_MYR2_R1_5M.fastq
gzip MC_MYR2_R2_5M.fastq
```

## Preparing for Alignment
```
# Install hisat 
conda install -c bioconda -y hisat2

mkdir ~/project_ngs1/hisat_align/hisat_index && cd ~/project_ngs1/hisat_align/hisat_index
ln -s ~/workdir/project_ngs1/sample_data/Danio_rerio.GRCz11.dna.chromosome.10.fa .

# Indexing 
hisat2_extract_splice_sites.py ~/workdir/project_ngs1/sample_data/Danio_rerio.GRCz11.99.chr.gtf > splicesites.tsv
hisat2_extract_exons.py ~/workdir/project_ngs1/sample_data/Danio_rerio.GRCz11.99.chr.gtf > exons.tsv 
hisat2-build -p 1 --ss splicesites.tsv --exon exons.tsv Danio_rerio.GRCz11.dna.chromosome.10.fa Danio_rerio.GRCz11
```
# Alignment
```
cd ~/workdir/project_ngs1/hisat_align
R1="$HOME/workdir/project_ngs1/sample_data/MC_MYR2_R1_5M.fastq.gz"
R2="$HOME/workdir/project_ngs1/sample_data/MC_MYR2_R2_5M.fastq.gz"
hisat2 -p 1 -x hisat_index/Danio_rerio.GRCz11 --dta --rna-strandness RF -1 $R1 -2 $R2 -S MC_MYR2_align.sam
```

# Prepare the SAM & BAM files
```
# install Samtools
conda install -y samtools
# convert the SAM file into BAM file 
samtools view -bS MC_MYR2_align.sam > MC_MYR2_align.bam
#convert the BAM file to a sorted BAM file. 
samtools sort MC_MYR2_align.bam -o MC_MYR2_align.sorted.bam
```
## Assembly
```
# install stringtie
conda install -y stringtie

# Assembly without known annotations
stringtie MC_MYR2_align.sorted.bam --rf -l ref_free -o ref_free.gtf
# No. of transcript
cat ref_free.gtf | grep -v "^@" | awk '$3=="transcript"' | wc -l

# Assembly with known previous annotations
stringtie MC_MYR2_align.sorted.bam --rf -l ref_sup -G ~/workdir/project_ngs1/sample_data/Danio_rerio.GRCz11.99.chr.gtf -o ref_sup.gtf
# No. of transcript
cat ref_sup.gtf | grep -v "^@" | awk '$3=="transcript"' | wc -l
``

# Secondry Alignment
``
cd ~/workdir/project_ngs1/hisat_align
R1="$HOME/workdir/project_ngs1/sample_data/MC_MYR2_R1_5M.fastq.gz"
R2="$HOME/workdir/project_ngs1/sample_data/MC_MYR2_R2_5M.fastq.gz"
hisat2 -p 1 -x hisat_index/Danio_rerio.GRCz11 --dta --rna-strandness RF -1 $R1 -2 $R2 --secondary -S MC_MYR2_sec_align.sam
```

# Prepare the SAM & BAM files for 2ry
```
# convert the SAM file into BAM file 
samtools view -bS MC_MYR2_sec_align.sam > MC_MYR2_sec_align.bam
```
#convert the BAM file to a sorted BAM file. 
samtools sort MC_MYR2_sec_align.bam -o MC_MYR2_sec_align.sorted.bam
```
## Assembly
```
# Assembly without known annotations
stringtie MC_MYR2_sec_align.sorted.bam --rf -l ref_free -o ref_free_sec.gtf
# No. of transcript
cat ref_free_sec.gtf | grep -v "^@" | awk '$3=="transcript"' | wc -l

# Assembly with known previous annotations
stringtie MC_MYR2_sec_align.sorted.bam --rf -l ref_sup -G ~/workdir/project_ngs1/sample_data/Danio_rerio.GRCz11.99.chr.gtf -o ref_sup_sec.gtf
# No. of transcript
cat ref_sup_sec.gtf | grep -v "^@" | awk '$3=="transcript"' | wc -l
```
## Comparing GTF files of 1ry & 2ry Alignment
 ```
# Install gffcompare
conda install gffcompare

# Run for ref free

mkdir -p ~/workdir/project_ngs1/gtf-compare/compare-free && cd ~/workdir/project_ngs1/gtf-compare/compare-free
gffcompare -r ~/workdir/project_ngs1/hisat_align/ref_free.gtf ~/workdir/project_ngs1/hisat_align/ref_free_sec.gtf

# Run for ref sup
mkdir -p ~/workdir/project_ngs1/gtf-compare/compare-sup && cd ~/workdir/project_ngs1/gtf-compare/compare-sup
gffcompare -r ~/workdir/project_ngs1/hisat_align/ref_sup.gtf ~/workdir/project_ngs1/hisat_align/ref_sup_sec.gtf
 ```




