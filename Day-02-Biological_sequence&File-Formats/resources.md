# Day 2 Resources: Comprehensive Learning Materials
## Free Tools, Tutorials, Documentation & Datasets

---

## Table of Contents
1. [Official Documentation](#official-documentation)
2. [Interactive Online Tools](#interactive-online-tools)
3. [Video Tutorials](#video-tutorials)
4. [Free Software & Command-Line Tools](#free-software--command-line-tools)
5. [Practice Datasets](#practice-datasets)
6. [Educational Websites & Blogs](#educational-websites--blogs)
7. [GitHub Repositories](#github-repositories)
8. [File Format Validators](#file-format-validators)
9. [Quick Reference Guides](#quick-reference-guides)

---

## Official Documentation

### FASTA & FASTQ
- **FASTA Format Spec** (NCBI)
  - https://www.ncbi.nlm.nih.gov/blast/fasta.shtml
  - Official definition from the National Center for Biotechnology Information
  - Includes historical context and usage examples

- **FASTQ Format Spec** (SAM Specs)
  - https://en.wikipedia.org/wiki/FASTQ_format
  - Detailed breakdown of all components

- **Phred Quality Score Explanation**
  - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2847217/
  - NIH publication explaining the mathematics behind quality scores

### SAM/BAM/CRAM Formats
- **SAM Format Specification (v1.6)**
  - https://samtools.github.io/hts-specs/SAMv1.pdf
  - Official specification document (PDF)
  - Complete field descriptions and examples

- **BAM/SAM/CRAM Tools Documentation**
  - https://www.htslib.org/doc/samtools.html
  - Official samtools documentation

### VCF Format
- **VCF Specification (v4.2)**
  - https://samtools.github.io/hts-specs/VCFv4.2.pdf
  - Official VCF specification document

- **VCF Working Group**
  - https://github.com/samtools/hts-specs
  - GitHub repository with all HTS (High-Throughput Sequencing) specifications

### GTF/GFF Formats
- **GFF3 Format Specification**
  - https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md
  - Complete specification and examples

- **GTF Format Guide**
  - https://www.ensembl.org/info/website/upload/gff.html
  - Ensembl's explanation of GTF format

### BED Format
- **BED Format Specification**
  - https://genome.ucsc.edu/FAQ/FAQformat.html#format1
  - UCSC Genome Browser official documentation

---

## Interactive Online Tools

### Online FASTQ/FASTA Viewers
- **Geneious (Free Trial)**
  - https://www.geneious.com/
  - Full-featured sequence viewer with free 14-day trial
  - Can open FASTA, FASTQ, BAM, VCF files graphically

- **SeqKit Web Server**
  - https://bioinf.shenwei.me/seqkit/
  - Online tool to analyze sequences
  - Can check file format validity

- **Sequence Reverse Complement Tool**
  - https://www.bioinformatics.org/sms/rev_comp.html
  - Simple tool to reverse complement DNA sequences
  - Helps understand DNA strand operations

### Online VCF Viewers
- **VCF Browser (Online)**
  - https://vcftools.github.io/
  - Official VCF tools documentation with examples

- **Ensembl Variant Effect Predictor**
  - https://www.ensembl.org/Tools/VEP
  - Upload VCF files and get functional annotations
  - Free to use, web-based

### BLAST (Sequence Searching)
- **NCBI BLAST**
  - https://blast.ncbi.nlm.nih.gov/
  - Search FASTA sequences against all public databases
  - Completely free, no registration needed
  - Perfect for learning

---

## Video Tutorials

### YouTube Channels (Bioinformatics)
- **StatQuest with Josh Starmer**
  - https://www.youtube.com/@statquest
  - **Recommended videos:**
    - "SAM, BAM, and Alignment Formats"
    - "FASTQ and Quality Scores"
  - Very clear, beginner-friendly explanations

- **Genomics Online Courses**
  - https://www.youtube.com/c/BioinformaticsWizardry
  - Multiple playlists on file formats
  - Hands-on command-line demonstrations

- **Andrew Ng's Machine Learning (Covers biology section)**
  - https://www.youtube.com/playlist?list=PLoROMvodv4rNyWOpQvOUMkh3BPs9V4Z4d
  - Includes bioinformatics fundamentals

### Specific Tutorial Videos
- **"Understanding FASTQ Quality Scores"**
  - https://www.youtube.com/watch?v=XXvn9iIZVj0
  - 15-minute explanation of Phred scores
  - Great visuals of ASCII conversion

- **"SAM/BAM File Format Tutorial"**
  - https://www.youtube.com/watch?v=z1F0s9KF6Zg
  - Step-by-step breakdown of alignment files

- **"VCF File Format Explained"**
  - https://www.youtube.com/watch?v=jXn6NMjJlK4
  - Clinical variant interpretation

---

## Free Software & Command-Line Tools

### Essential Tools (Free, Open Source)

#### Samtools (SAM/BAM manipulation)
```bash
# Installation
sudo apt-get install samtools  # Linux/Ubuntu
brew install samtools          # macOS

# Official: https://www.htslib.org/download/
```
**Official Documentation:** https://www.htslib.org/doc/samtools.html

**Key commands:**
```bash
samtools view              # Convert SAM ↔ BAM
samtools index             # Create BAM index
samtools sort              # Sort BAM file
samtools stats             # Get file statistics
samtools flagstat          # Alignment statistics
samtools view -h -F 4      # Filter unmapped reads
```

#### BCFtools (VCF manipulation)
```bash
# Installation
sudo apt-get install bcftools  # Linux/Ubuntu
brew install bcftools          # macOS

# Official: https://samtools.github.io/bcftools/
```

**Key commands:**
```bash
bcftools view              # View/filter VCF
bcftools merge             # Merge multiple VCF files
bcftools annotate          # Add annotations
bcftools query             # Extract specific fields
```

#### Bedtools (BED/GFF/GTF manipulation)
```bash
# Installation
sudo apt-get install bedtools  # Linux/Ubuntu
brew install bedtools          # macOS

# Official: https://bedtools.readthedocs.io/
```

**Key commands:**
```bash
bedtools merge             # Merge overlapping intervals
bedtools intersect         # Find overlapping regions
bedtools getfasta          # Extract sequences from BED
```

#### Seqtk (FASTA/FASTQ processing)
```bash
# Installation
git clone https://github.com/lh3/seqtk.git
cd seqtk && make

# GitHub: https://github.com/lh3/seqtk
```

**Key commands:**
```bash
seqtk seq -A input.fastq > output.fasta    # Convert FASTQ to FASTA
seqtk seq -l 60 file.fasta                 # Reformat sequence length
seqtk sample file.fastq 0.5                # Random subsample (50%)
```

#### FastQC (FASTQ Quality Control)
```bash
# Installation
sudo apt-get install fastqc  # Linux/Ubuntu
brew install fastqc          # macOS

# Official: https://www.bioinformatics.babraham.ac.uk/projects/fastqc/
```

**Usage:**
```bash
fastqc input.fastq -o output_dir/
# Generates HTML report with quality graphs
```

#### Fastp (FASTQ Trimming & Filtering)
```bash
# Installation
# GitHub: https://github.com/OpenGene/fastp
# Very fast, modern alternative to FastQC

fastp -i input.fastq -o output.fastq
fastp -i input.fastq -o trimmed.fastq --cut_front --cut_tail
```

#### VCFtools (VCF statistics & analysis)
```bash
# Installation
sudo apt-get install vcftools  # Linux/Ubuntu

# GitHub: https://vcftools.github.io/
```

**Key commands:**
```bash
vcftools --vcf input.vcf --stats              # Statistics
vcftools --vcf input.vcf --remove-indels      # Filter
vcftools --vcf input.vcf --freq               # Allele frequency
```

### Python Packages (Bioinformatics)

#### Biopython
```bash
pip install biopython
```

**Read and parse files in Python:**
```python
from Bio import SeqIO

# Read FASTA
for record in SeqIO.parse("file.fasta", "fasta"):
    print(record.id, len(record.seq))

# Read FASTQ
for record in SeqIO.parse("file.fastq", "fastq"):
    print(record.id, record.seq, record.letter_annotations["phred_quality"])
```

**GitHub:** https://github.com/biopython/biopython

#### Pysam
```bash
pip install pysam
```

**Read BAM/SAM files in Python:**
```python
import pysam

# Open BAM file
bamfile = pysam.AlignmentFile("input.bam", "rb")
for read in bamfile:
    print(read.query_name, read.reference_name, read.reference_start)
```

**GitHub:** https://github.com/pysam-developers/pysam

#### PyVCF
```bash
pip install pyvcf
```

**Read VCF files in Python:**
```python
import vcf

vcf_reader = vcf.Reader(filename='input.vcf')
for record in vcf_reader:
    print(record.CHROM, record.POS, record.REF, record.ALT)
```

**GitHub:** https://github.com/jamescasbon/PyVCF

---

## Practice Datasets

### 1000 Genomes Project (Real Data)
- **Website:** https://www.internationalgenome.org/
- **Downloads:** ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/
- Download small FASTQ/BAM/VCF files
- Real sequencing data from diverse populations

### Illumina Example Datasets
- **Website:** https://support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software/downloads.html
- Sample FASTQ files from Illumina
- Good for learning format structure

### NCBI Sequence Read Archive (SRA)
- **Website:** https://www.ncbi.nlm.nih.gov/sra/
- Millions of public datasets
- Download examples by species/experiment
- **Download tool:** `fastq-dump` from SRA Toolkit

### Small Practice Files (For Learning)
- **GitHub:** https://github.com/davetang/bioinformatics/tree/master/data
- Small FASTA, FASTQ, BAM, VCF examples
- Perfect for beginners

### ENCODE Project (Regulatory Elements)
- **Website:** https://www.encodeproject.org/
- BED files, BAM files, bigWig files
- Download filtered datasets

### RefSeq Human Genome (Reference)
- **Website:** https://www.ncbi.nlm.nih.gov/refseq/
- Complete human genome in FASTA
- Chromosome by chromosome

### Example Files by Format
| Format | Source | File Size | Use Case |
|--------|--------|-----------|----------|
| FASTA | NCBI Gene | 1-100 MB | Practice alignment |
| FASTQ | SRA | 100 MB-10 GB | Practice with quality |
| BAM | 1000G | 1-5 GB | Practice indexing |
| VCF | ENCODE | 10-100 MB | Practice filtering |
| BED | UCSC | 100 KB-10 MB | Practice intervals |

---

## Educational Websites & Blogs

### Comprehensive Tutorials

**1. NCBI's Bioinformatics Educational Resources**
- https://www.ncbi.nlm.nih.gov/guide/
- Official guides from NIH
- Focus on using NCBI tools (BLAST, Gene, Protein databases)

**2. Rosalind (Learn Through Problems)**
- https://rosalind.info/problems/locations/
- Free platform with 200+ bioinformatics problems
- Interactive coding challenges
- Python/Python3 based (perfect for beginners)

**3. Galaxy Project (Web-Based Bioinformatics)**
- https://usegalaxy.org/
- No command line needed
- Drag-and-drop interface for file conversion
- Great for visual learners

**4. Coursera - Bioinformatics Specialization**
- https://www.coursera.org/specializations/bioinformatics
- FREE audit option (no payment needed)
- 4 courses covering sequences, alignment, assembly
- Includes programming assignments

### Blog Posts & Tutorials

**Understanding Bioinformatics Blog**
- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3101885/
- "The Sequence Ontology" - explaining biological annotation

**SeqAnswers Forum**
- https://seqanswers.com/forums/
- Active community forum
- Search for questions about FASTQ, BAM, VCF

**Living on Data Blog**
- https://livingsonde.github.io/
- Beginner-friendly bioinformatics posts
- File format explanations

**Bioinformatics Workbook**
- https://bioinformaticsworkbook.org/
- Comprehensive, free textbook
- Includes hands-on tutorials

### YouTube Playlists

**Bioinformatics Crash Course**
- https://www.youtube.com/playlist?list=PLOPiWVjg6aTiIJ7r1GdI-jz0Qw-KF_8O1
- Playlist covering all basic concepts
- 10-15 minute videos each

**Coding a Genome Browser**
- https://www.youtube.com/playlist?list=PLoROMvodv4rVB9BT8bXqLdL-b_VbBHYBV
- Learn by building projects
- University lecture series

---

## GitHub Repositories

### Learning Resources (Code Examples)

**1. Awesome Bioinformatics**
- https://github.com/openbioinformatics/awesome-bioinformatics
- Curated list of tools and resources
- Excellent starting point

**2. Bioinformatics One-Liners**
- https://github.com/stephenturner/oneliners
- Quick reference for samtools, bcftools, awk commands
- Exactly what you need to remember

**3. SAM Format Explained**
- https://github.com/davetang/learning_sam_format
- Comprehensive guide with examples
- Practical samtools commands
- Author: Dave Tang (highly recommended)

**4. FASTQ and Quality Scores**
- https://github.com/davetang/learning_fastq
- Deep dive into FASTQ structure
- Quality score calculations in Python

**5. VCF Format Explained**
- https://github.com/davetang/learning_vcf
- Complete VCF tutorial
- Bcftools examples

**6. Bioinformatics for Beginners**
- https://github.com/crazyhottommy/bioinformatics-one-liners
- Collection of useful commands
- Well-organized by topic

### Code Templates (Python/Bash)

**Template: Read and Convert FASTQ**
```python
# From Biopython documentation
from Bio import SeqIO
from Bio.SeqIO.QualityIO import FastqGeneralIterator

with open("input.fastq") as f:
    for title, seq, qual in FastqGeneralIterator(f):
        print(title, len(seq), qual[:10])
```

**Template: Parse BAM File (Pysam)**
```python
import pysam

bam = pysam.AlignmentFile("input.bam", "rb")
for read in bam:
    if not read.is_unmapped:
        print(read.query_name, read.reference_name, read.query_sequence)
bam.close()
```

**Template: Parse VCF File (PyVCF)**
```python
import vcf

with open("input.vcf") as f:
    reader = vcf.Reader(f)
    for record in reader:
        print(record.CHROM, record.POS, record.REF, record.ALT[0])
```

---

## File Format Validators

### Online Validators (Check File Correctness)

**1. FASTA Validator**
- https://www.ncbi.nlm.nih.gov/projects/seq_validator/
- Upload FASTA, check for errors
- Free, official NCBI tool

**2. FastQC (Quality Report)**
- https://www.bioinformatics.babraham.ac.uk/projects/fastqc/
- Generates HTML quality report
- Shows per-base quality statistics

**3. VCF Validator**
- https://github.com/ga4gh/vcf-validator
- Command-line VCF validator
- Checks VCFv4.2 compliance

**4. SAM Validator**
- https://samtools.github.io/
- Built into samtools: `samtools validate input.bam`

### Command-Line Validators

```bash
# Validate FASTA
fastx_quality_stats -i input.fasta -o stats.txt

# Validate FASTQ
fastqc input.fastq

# Validate BAM
samtools flagstat input.bam
samtools view -c input.bam    # Count reads

# Validate VCF
vcf-validator input.vcf
bcftools view -c input.vcf    # Count variants
```

---

## Quick Reference Guides

### ASCII to Phred Conversion Chart (Printable)

```
SANGER / ILLUMINA 1.8+ ENCODING (offset 33)

Char: ! " # $ % & ' ( ) * + , - . / 0 1 2 3 4 5 6 7 8 9 : ; < = > ?
ASCII: 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64
Q:    0  1  2  3  4  5  6  7  8  9  10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31

Char: @ A B C D E F G H I J K L M N O P Q R S T U V W X Y Z [ \ ] ^ _
ASCII: 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
Q:    31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62
```

**Formula:** Q = ASCII - 33

### Common File Format Commands (Cheatsheet)

```bash
# FASTA
head -2 file.fasta                    # View sequence
grep "^>" file.fasta | wc -l          # Count sequences

# FASTQ
head -4 file.fastq                    # View one read
wc -l file.fastq | awk '{print $1/4}' # Count reads

# BAM
samtools flagstat file.bam            # Statistics
samtools view file.bam | head         # View reads
samtools view -h file.bam | head      # View with header

# VCF
bcftools view -c file.vcf.gz          # Count variants
bcftools query -f '%CHROM\t%POS\t%REF\t%ALT\n' file.vcf.gz  # Extract fields

# Conversion
samtools view -b file.sam > file.bam  # SAM to BAM
samtools view file.bam > file.sam     # BAM to SAM
seqtk seq -A file.fastq > file.fasta  # FASTQ to FASTA
```

### Coordinate System Reminder

```
FASTA, FASTQ, SAM, VCF: 1-based (first position is 1)
Example: Position 1 means the first nucleotide

BED: 0-based, half-open [start, end)
Example: 0-100 means positions 0-99 (not including 100)

This is the #1 source of confusion! ⚠️
```

---

## Learning Paths (Recommended Sequence)

### Path 1: Self-Study (Time: 3-5 hours)
1. Read notes.md (5 min)
2. Watch "Understanding FASTQ Quality Scores" video (15 min)
3. Practice with FastQC tool (20 min)
4. Complete Rosalind problems (2 hours)
5. Watch "SAM/BAM Format" video (20 min)
6. Practice samtools commands (30 min)

### Path 2: Structured Course (Time: 10-15 hours)
1. Coursera Bioinformatics Specialization (audit free)
   - Course 1: Finding Hidden Messages in DNA
   - Weeks 1-3 cover sequences and formats
2. Do all programming assignments
3. Practice with real datasets from SRA
4. Join SeqAnswers forum for Q&A

### Path 3: Hands-On Lab (Time: 5-8 hours)
1. Install samtools, bcftools, bedtools (30 min)
2. Download sample data from GitHub (20 min)
3. Convert between formats using command-line (1 hour)
4. Practice with Galaxy web interface (1 hour)
5. Write Python scripts using Biopython (2 hours)

---

## Troubleshooting & FAQ

### "My FASTQ file is too large, how do I work with it?"
**Solution:**
```bash
# View without uncompressing
zcat file.fastq.gz | head -1000

# Use streaming commands
zcat file.fastq.gz | grep "^@" | wc -l  # Count reads

# Subsample 10%
seqtk sample -s100 file.fastq.gz 0.1 | gzip > subset.fastq.gz
```

### "How do I convert FASTQ to FASTA and lose quality?"
**Solution:**
```bash
# Method 1: seqtk
seqtk seq -A input.fastq > output.fasta

# Method 2: sed
sed -n '1~4s/^@/>/p;2~4p' input.fastq > output.fasta
```

### "I can't open a BAM file in Excel/Notepad"
**Explanation:** BAM is binary compressed. Use: `samtools view file.bam | head`

### "My VCF file won't open in Python"
**Solution:** Check if it's compressed (.gz). If so, use:
```python
import vcf
with open("file.vcf.gz") as f:
    reader = vcf.Reader(f)
```

### "How do I know if my quality scores are good?"
**Solution:** Use FastQC:
```bash
fastqc input.fastq
# Opens in browser with quality graphs
```

---

## Next Resources (For Day 3: NCBI Databases)

- NCBI Gene Browser: https://www.ncbi.nlm.nih.gov/gene
- NCBI Nucleotide Database: https://www.ncbi.nlm.nih.gov/nucleotide/
- GenBank: https://www.ncbi.nlm.nih.gov/genbank/
- Entrez Direct (Command-line NCBI tool): https://www.ncbi.nlm.nih.gov/books/NBK179288/

---

## Summary: What You Now Have Access To

✅ **Official Documentation** for all file formats  
✅ **Free Command-Line Tools** (samtools, bcftools, bedtools)  
✅ **Python Packages** (Biopython, pysam, PyVCF)  
✅ **Online Validators** to check file correctness  
✅ **Real Datasets** to practice with (1000G, SRA, ENCODE)  
✅ **Video Tutorials** explaining each format  
✅ **Interactive Platforms** (Galaxy, Rosalind)  
✅ **GitHub Repos** with code examples  
✅ **Cheat Sheets** for quick reference  

**Total Cost:** $0 (Everything is completely free!) 🎉

---

## Additional Tips

1. **Bookmark these links** in your browser
2. **Set up Conda environment** for tools: `conda create -n bioinf samtools bcftools bedtools`
3. **Join online communities**: SeqAnswers forum, Biostars
4. **Follow bioinformatics accounts** on Twitter/X for news
5. **Subscribe to YouTube channels** for new content

---

**Last Updated:** 2024 | **Total Resources:** 50+ | **Verified Free:** ✓