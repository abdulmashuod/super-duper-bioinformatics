# Bioinformatics File Formats: Comprehensive Guide

## Table of Contents
1. [FASTA Format](#fasta-format)
2. [FASTQ Format](#fastq-format)
3. [SAM Format](#sam-format)
4. [BAM Format](#bam-format)
5. [VCF Format](#vcf-format)
6. [Additional Formats](#additional-formats)
7. [Format Comparison](#format-comparison)
8. [Tools for Format Conversion](#tools-for-format-conversion)

---

## FASTA Format

### Definition
**FASTA** (Fast All) is the simplest and most universal biological sequence format. It's plain text-based and can store DNA, RNA, or protein sequences.

### Format Structure
```
>sequence_header [optional description]
sequence_line_1
sequence_line_2
sequence_line_n

>sequence_header_2
sequence_line_1
sequence_line_2
```

### Detailed Components

#### Header Line (starts with `>`)
- **Format**: `>sequence_id [optional description]`
- **NCBI convention**: `>gi|12345|ref|NC_000001.1| Homo sapiens chromosome 1`
- **UniProt convention**: `>sp|P69905|HBA_HUMAN Hemoglobin subunit alpha`
- Everything after the first space is optional description
- Must be on a single line

#### Sequence Lines
- Contain biological sequence (A, T, G, C for DNA; U for RNA; 20 letter amino acid codes for proteins)
- Can be any length (typically 50-80 characters per line for readability)
- Case-insensitive (ATGC = atgc)
- Whitespace is ignored
- Can include ambiguity codes: N (any nucleotide), R (purine), Y (pyrimidine), etc.

### Complete Example
```
>NM_001005484 Homo sapiens hemoglobin beta (HBB), mRNA
CCCTTGCTCCTCTCCTAGGACACCCAAGGTCAGGAGGCCAATATTTTAGGTACTGGT
TAGTATTGGATGTGAGTGCTGACTGACTACCTGAGAGCTGTGGAGTTCAGGGTGAGG
CCCTTGACTCCACAGACACCCAAGACCACCAGACCTGGGCAACGTGCTGGAGATGCAC
```

### Advantages
- Universal compatibility (every bioinformatics tool reads FASTA)
- Human-readable
- Small file size
- Simple to parse

### Disadvantages
- No quality information
- No metadata fields
- Ambiguous for multi-line sequences

### Use Cases
- Reference genomes
- Gene databases (RefSeq, GenBank)
- Sequence searching (BLAST)
- Protein sequences (UniProt)
- Storing sequences for alignment

### File Extensions
- `.fasta`, `.fa`, `.faa` (proteins), `.fna` (nucleic acids)

---

## FASTQ Format

### Definition
**FASTQ** (Fast Quality) extends FASTA by adding **quality scores** for each nucleotide. It's the standard output format from modern DNA sequencers (Illumina, Ion Torrent, PacBio).

### Format Structure
```
@sequence_id [optional description]
sequence
+[sequence_id or empty]
quality_scores

@sequence_id_2
sequence
+
quality_scores
```

### Detailed Components

#### Line 1: Header (starts with `@`)
- Similar to FASTA but uses `@` instead of `>`
- **Illumina format**: `@HWUSI-EAS100R:6:73:941:1973#GCCAAT/1`
  - HWUSI-EAS100R = instrument ID
  - 6 = flow cell lane
  - 73 = tile
  - 941:1973 = X:Y coordinates
  - #GCCAAT = barcode
  - /1 = read in pair (1 or 2)

#### Line 2: Sequence
- DNA bases (ATCG, N for unknown)
- Same format as FASTA sequence line
- Always single line in FASTQ

#### Line 3: Separator (starts with `+`)
- Usually just `+`
- Optionally repeats the sequence ID
- For legacy compatibility sometimes includes `+sequence_id`

#### Line 4: Quality Scores
- **Critical component**: ASCII-encoded quality score for each base
- **Length must equal sequence length** (one character per base)
- Each character represents a Phred Quality Score

### Understanding Phred Quality Scores (Q-values)

#### What is a Phred Score?
The Phred Quality Score is a logarithmic measure of base-calling accuracy:

```
Q = -10 × log₁₀(P_error)
```

Where `P_error` = probability the base is incorrect

#### Quality Score to Accuracy Conversion Table
```
Q Score | Error Probability | Accuracy  | Interpretation
--------|------------------|-----------|------------------
Q10     | 0.1 (1 in 10)    | 90.0%     | Poor quality
Q20     | 0.01 (1 in 100)  | 99.0%     | Acceptable quality
Q25     | 0.003 (1 in 316) | 99.7%     | Good quality
Q30     | 0.001 (1 in 1000)| 99.9%     | Excellent quality
Q40     | 0.0001           | 99.99%    | Exceptional quality (rare)
```

#### ASCII Encoding: Converting Character to Q-value

**The Key Formula:**
```
Q_value = ASCII_value - 33

(for Sanger/Illumina 1.8+ standard)
```

**Step-by-Step Calculation:**

1. **Identify the ASCII character** from quality line (e.g., `I`, `#`, `A`)

2. **Get its ASCII decimal value**:
   ```
   Character: I → ASCII decimal: 73
   Character: # → ASCII decimal: 35
   Character: A → ASCII decimal: 65
   ```

3. **Subtract 33** (Sanger/Illumina offset):
   ```
   Q_I = 73 - 33 = 40 (99.99% accuracy - excellent!)
   Q_# = 35 - 33 = 2  (63% accuracy - very poor!)
   Q_A = 65 - 33 = 32 (99.968% accuracy - very good!)
   ```

#### ASCII Quality Score Mapping (Sanger/Illumina 1.8+)

```
ASCII Character: ! " # $ % & ' ( ) * + , - . / 0 1 2 3 4 5 6 7 8 9 : ; < = > ?
ASCII Value:     33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64
Q Value:         0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31

ASCII Character: @ A B C D E F G H I J K L M N O P Q R S T U V W X Y Z [ \ ] ^ _
ASCII Value:     64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
Q Value:         31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62
```

#### Common Quality Characters (Sanger Encoding)

```
I (ASCII 73) = Q40 → 99.99% accurate (BEST)
H (ASCII 72) = Q39 → 99.97% accurate
G (ASCII 71) = Q38 → 99.84% accurate
F (ASCII 70) = Q37 → 99.80% accurate
E (ASCII 69) = Q36 → 99.75% accurate
D (ASCII 68) = Q35 → 99.68% accurate
5 (ASCII 53) = Q20 → 99.00% accurate
0 (ASCII 48) = Q15 → 96.84% accurate
# (ASCII 35) = Q2  → 63% accurate (WORST)
! (ASCII 33) = Q0  → 0% confidence
```

### Complete FASTQ Example with Quality Analysis

```fastq
@SRR001666.1 071112_SOLEXADEV_s_1_sequence sequence  
GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTAAATCCATTTGTTCAACTCACAGTGT
+
!''*((((***+))%%%++)(%%%%).1***-+*''))**55CCF>>>>>>CCCCCCC65
```

**Quality score breakdown:**
```
Base:    G A T T T G G G G T T C A A A G C A G T A T C G A T C A A A T A G T A A A T C C A T T T G T T C A A C T C A C A G T G T
Quality: ! ' ' * ( ( ( ( * * * + ) ) % % % + + ) ( % % % % ) . 1 * * * - + * ' ' ) ) * * 5 5 C C F > > > > > > C C C C C C C 6 5
Q-value: 0 6 6 10 8 8 8 8 10 10 10 11 9 9 5 5 5 11 11 9 8 5 5 5 5 9 14 16 10 10 10 3 11 10 6 6 9 9 10 10 21 21 34 34 37 30 30 30 30 30 30 34 34 34 34 34 34 34 6 21
```

**Interpretation:** Quality improves from start (mostly < Q10) to middle (Q20-37) then drops toward end (Q6).

### FASTQ Quality Standards
- **Q ≥ 30**: Typically considered "high quality" for downstream analysis
- **Q 20-29**: Moderate quality, may be used with filtering
- **Q < 20**: Low quality, often trimmed or discarded

### Advantages
-  Includes quality information (critical for variant calling)
-  Standard output from all modern sequencers
-  Widely supported by bioinformatics tools
-  Allows quality-based filtering and trimming

### Disadvantages
- Large file sizes (3-4x larger than FASTA)
- Requires more storage and bandwidth
- Quality scores only for short reads (not applicable to assembled sequences)

### Use Cases
- Raw sequencing data (primary output from sequencers)
- Quality control and trimming
- Assembly algorithms (use quality scores to guide)
- Error correction in sequencing reads

### File Extensions
- `.fastq`, `.fq`
- `.fastq.gz`, `.fq.gz` (compressed, very common)

---

## SAM Format

### Definition
**SAM** (Sequence Alignment Map) is a text-based format for storing sequence alignments. It maps reads back to a reference genome.

### Format Structure
```
@HD	VN:1.6	SO:coordinate
@SQ	SN:chr1	LN:249250621
@PG	ID:bwa	PN:bwa	VN:0.7.17
read_id	163	chr1	1000	60	100M	=	1500	600	ATGCATGC...	IIIIIIII...	RG:Z:sample1
```

### Header Section (starts with `@`)

#### @HD (Header line)
- `VN:1.6` = Version
- `SO:coordinate/queryname/unsorted` = Sort order

#### @SQ (Sequence Dictionary)
- `SN:chr1` = Sequence name
- `LN:249250621` = Sequence length

#### @PG (Program)
- `ID:bwa` = Program identifier
- `PN:bwa` = Program name
- `VN:0.7.17` = Version

### Alignment Section (Tab-separated)

11 mandatory fields:
```
QNAME    FLAG      RNAME   POS  MAPQ  CIGAR   RNEXT   PNEXT   TLEN    SEQ     QUAL    [Optional tags]
```

| Field | Description |
|-------|-------------|
| QNAME | Read name/ID |
| FLAG | Bitwise flag (unmapped, reverse complement, paired, etc.) |
| RNAME | Reference sequence name (chromosome) |
| POS | 1-based leftmost alignment position |
| MAPQ | Mapping quality (0-60, similar to Phred) |
| CIGAR | Alignment string (100M = 100 matches, 5D = 5 deletions) |
| RNEXT | Mate reference name (= means same as RNAME) |
| PNEXT | Mate alignment position |
| TLEN | Template length (insert size) |
| SEQ | Query sequence |
| QUAL | Query quality scores (FASTQ format) |
| Tags | Optional fields (RG:Z:sample1, AS:i:50, etc.) |

### FLAG Field Explanation (Bitwise)
```
Bit  Value  Meaning
0    1      Read is mapped in proper pair
1    2      Read unmapped
2    4      Mate unmapped
3    8      Read reverse strand
4    16     Mate reverse strand
5    32     First in pair
6    64     Second in pair
7    128    Secondary alignment
8    256    QC failure
9    512    PCR duplicate
```

### Example SAM Record
```
SRR001666.1	163	chr1	1000	60	100M	=	1500	600	GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTAAATCCATTTGTTCAACTCACAGTGT	I''*((((***+))%%%++)(%%%%).1***-+*''))**55CCF>>>>>>CCCCCCC65	RG:Z:sample1	AS:i:100	NM:i:0
```

### Advantages
- Standardized format for alignment
- Contains quality scores and alignment details
- Text-based, human-readable
- Widely used in bioinformatics pipelines

### Disadvantages
-  Large file sizes
-  Slow to process (not indexed)
-  Redundant storage (qualities and reads repeated)

---

## BAM Format

### Definition
**BAM** (Binary Alignment Map) is the binary, compressed version of SAM. It's identical information but ~10-20x smaller.

### Key Differences from SAM
| Aspect | SAM | BAM |
|--------|-----|-----|
| Format | Text | Binary (compressed) |
| File Size | Large | 10-20x smaller |
| Human-Readable | Yes | No |
| Processing Speed | Slow | Fast |
| Indexing | Not indexed | Indexed (.bai) |
| Streaming | Can stream | Requires indexing |

### BAM File Structure
```
[BAM Magic String: BAM\1]
[Gzip compressed header]
[Gzip compressed reads]
[Index (optional .bai file)]
```

### Creating and Using BAM Files
```bash
# SAM to BAM conversion
samtools view -b input.sam > output.bam

# Index BAM file (creates output.bam.bai)
samtools index output.bam

# Quick lookup at specific region
samtools view output.bam chr1:1000-2000

# Sort BAM file
samtools sort input.bam -o sorted.bam
```

### Advantages
- ✅ Massive storage savings (crucial for large datasets)
- ✅ Fast random access (via index)
- ✅ Standard format for NGS analysis
- ✅ Widely supported

### Disadvantages
- ❌ Binary format (can't view directly)
- ❌ Requires tools to read
- ❌ Requires indexing for random access

### Use Cases
- Storage of alignment results
- Variant calling pipelines
- SNP and indel detection
- Coverage analysis

### File Extensions
- `.bam` (main file)
- `.bai` (index file)
- `.cram` (even more compressed, similar to BAM)

---

## VCF Format

### Definition
**VCF** (Variant Call Format) stores genetic variations (SNPs, indels, structural variants) found by comparing sequences to a reference genome.

### Format Structure
```
##fileformat=VCFv4.2
##fileDate=20230101
##reference=GRCh38
##INFO=<ID=DP,Number=1,Type=Integer,Description="Read Depth">
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO	FORMAT	sample1	sample2
chr1	14370	rs6054257	G	A	29	PASS	NS=3;DP=14;AF=0.5;DB	GT:GQ	0/1:48	0/1:48
chr1	17330	.	T	A	3	q10	NS=3;DP=11;AF=0.017	GT:GQ	0/0:49	0/0:49
```

### Header Section (lines starting with `##` and `#`)

| Header | Description |
|--------|-------------|
| `##fileformat` | VCF format version (required) |
| `##fileDate` | Date file was created |
| `##reference` | Reference genome used |
| `##INFO` | Metadata about INFO fields |
| `##FORMAT` | Metadata about sample format fields |
| `#CHROM...` | Column headers (tab-separated) |

### Data Section (8 fixed columns + sample columns)

| Column | Name | Description |
|--------|------|-------------|
| 1 | CHROM | Chromosome name |
| 2 | POS | 1-based position |
| 3 | ID | SNP identifier (rsID) or . |
| 4 | REF | Reference allele (a-z, A-Z, . or *) |
| 5 | ALT | Alternate allele(s), comma-separated |
| 6 | QUAL | Phred quality score for variant |
| 7 | FILTER | PASS if passes all filters, else filter name |
| 8 | INFO | Additional information (semicolon-separated) |
| 9+ | FORMAT | Format string for sample data |
| 10+ | Sample Data | Individual sample genotype information |

### Common INFO Fields
```
DP      = Total read depth
AF      = Allele frequency
MQ      = Mapping quality
AN      = Total number of alleles
AC      = Allele count
DB      = Variant in dbSNP
SOMATIC = Somatic mutation (cancer)
CSQ     = VEP consequence annotations
```

### Common FORMAT Fields
```
GT  = Genotype (0/1 = heterozygous, 0/0 = homozygous ref, 1/1 = homozygous alt)
GQ  = Genotype quality
DP  = Read depth at this site
AD  = Allele depth (ref,alt)
PL  = Phred-scaled likelihood
```

### Complete VCF Example
```vcf
##fileformat=VCFv4.2
##fileDate=20230115
##reference=GRCh38/hg38
##INFO=<ID=DP,Number=1,Type=Integer,Description="Total Depth">
##INFO=<ID=AF,Number=A,Type=Float,Description="Allele Frequency">
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=GQ,Number=1,Type=Integer,Description="Genotype Quality">
##FORMAT=<ID=DP,Number=1,Type=Integer,Description="Read Depth">
#CHROM	POS	ID	REF	ALT	QUAL	FILTER	INFO	FORMAT	patient1	patient2
chr1	14370	rs6054257	G	A	29	PASS	DP=14;AF=0.5	GT:GQ:DP	0/1:48:10	0/0:49:8
chr1	17330	.	T	A	3	q10	DP=11;AF=0.017	GT:GQ:DP	0/0:49:8	0/0:46:7
chr1	1110696	rs6040355	A	G,T	67	PASS	DP=10;AF=0.333,0.667	GT:GQ:DP	1/2:21:6	2/2:35:4
chrX	10	.	T	A	40	PASS	DP=13	GT:GQ	0/0:18	./.	
```

### Interpretation Examples
```
0/0 = Homozygous reference (both alleles are reference)
0/1 = Heterozygous (one reference, one alternate)
1/1 = Homozygous alternate (both alleles are alternate)
./. = Missing genotype data
0/2 = Two different alternate alleles (rare)
```

### Advantages
- ✅ Standard format for variant data
- ✅ Human-readable
- ✅ Supports multiple samples
- ✅ Flexible annotation (INFO field)
- ✅ Widely supported (all variant callers output VCF)

### Disadvantages
- ❌ Large file sizes for whole genomes
- ❌ Text-based (slow for large datasets)
- ❌ Can be complex with many INFO fields

### Use Cases
- Variant calling results
- SNP and indel detection
- Structural variant reporting
- Cancer mutation databases
- Clinical variant interpretation

### File Extensions
- `.vcf` (plain text)
- `.vcf.gz` (gzip compressed, standard)
- `.vcf.gz.tbi` (tabix index)

---

## Additional Formats

### GFF/GTF (Gene Feature Format / General Transfer Format)

**Purpose**: Genomic feature annotations (genes, exons, transcripts)

```
chr1	NCBI	gene	11869	14409	.	+	.	ID=gene-DDX11L1;Name=DDX11L1
chr1	NCBI	mRNA	11869	14409	.	+	.	ID=rna-NR_046018.2;Parent=gene-DDX11L1
chr1	NCBI	exon	11869	12227	.	+	.	ID=exon-NR_046018.2-1;Parent=rna-NR_046018.2
```

| Column | Content |
|--------|---------|
| 1 | Seqname (chromosome) |
| 2 | Source (database/tool) |
| 3 | Feature (gene, exon, CDS, etc.) |
| 4 | Start (1-based) |
| 5 | End |
| 6 | Score (. if not applicable) |
| 7 | Strand (+, -, or .) |
| 8 | Frame (0, 1, 2 for CDS) |
| 9 | Attributes (key=value pairs) |

### BED (Browser Extensible Data)

**Purpose**: Regions of genomic interest (peaks, features, segments)

```
chr1	1000	2000	peak1	100	+
chr1	5000	6500	peak2	200	-
chr2	3000	4000	peak3	150	+
```

| Column | Content |
|--------|---------|
| 1 | Chromosome |
| 2 | Start (0-based, IMPORTANT!) |
| 3 | End (exclusive, IMPORTANT!) |
| 4 | Name |
| 5 | Score |
| 6 | Strand |

**Critical**: BED uses 0-based, half-open coordinates (different from 1-based VCF!)

### GZIP Compressed Formats

Many formats are compressed with gzip to save space:
```
.fasta.gz    → FASTA compressed
.fastq.gz    → FASTQ compressed (very common)
.vcf.gz      → VCF compressed with tabix index (.vcf.gz.tbi)
.bam         → Already compressed (BAM = compressed SAM)
```

---

## Format Comparison Matrix

| Aspect | FASTA | FASTQ | SAM | BAM | VCF | GFF | BED |
|--------|-------|-------|-----|-----|-----|-----|-----|
| **Content** | Sequences | Sequences + Quality | Alignments | Alignments | Variants | Annotations | Regions |
| **Format** | Text | Text | Text | Binary | Text | Text | Text |
| **Size** | Small | Large | Very Large | Compressed | Medium | Medium | Small |
| **Quality Info** | No | Yes | Yes | Yes | Yes | No | No |
| **Indexable** | No | No | No | Yes (.bai) | Yes (.tbi) | No | Yes (.bgz) |
| **Speed** | Fast | Fast | Slow | Very Fast | Medium | Medium | Fast |
| **Primary Use** | Sequences | Raw reads | Mappings | Storage | Variants | Genes | Regions |

---

## Format Conversion Workflows

### Common Conversion Pipeline
```
Sequencing Machine
        ↓
      FASTQ (raw reads)
        ↓
    [QC & Trimming]
        ↓
    Alignment Software
        ↓
      SAM (text alignment)
        ↓
    samtools view -b
        ↓
      BAM (compressed alignment)
        ↓
    [Variant Calling]
        ↓
      VCF (variants found)
        ↓
    [Annotation]
        ↓
    VCF + GTF (annotated variants)
```

### Converting Between Formats

**FASTQ → FASTA**
```bash
# Remove quality scores
seqtk seq -A input.fastq > output.fasta
```

**SAM → BAM**
```bash
samtools view -b input.sam > output.bam
```

**BAM → SAM**
```bash
samtools view input.bam > output.sam
```

**Sort and Index BAM**
```bash
samtools sort input.bam -o sorted.bam
samtools index sorted.bam
```

**Compress VCF**
```bash
bgzip -c input.vcf > output.vcf.gz
tabix -p vcf output.vcf.gz
```

---

## Tools for Working with These Formats

### Universal Tools
- **samtools**: Read/write/convert SAM/BAM/CRAM files
- **bcftools**: Read/write/process VCF files
- **bedtools**: Manipulate BED, GFF, GTF files
- **seqtk**: FASTA/FASTQ manipulation

### Sequence Analysis
- **biopython**: Parse and manipulate sequences in Python
- **EMBOSS**: Sequence analysis suite
- **FastQC**: FASTQ quality assessment
- **fastp**: FASTQ trimming and filtering

### Alignment
- **BWA**: Short-read alignment (outputs SAM)
- **bowtie2**: Rapid alignment (outputs SAM)
- **STAR**: RNA-seq alignment

### Variant Calling
- **GATK**: Gold standard variant caller (outputs VCF)
- **FreeBayes**: Bayesian variant caller (outputs VCF)
- **SAMtools mpileup**: Simple variant caller

### File Inspection
```bash
# View first few lines
head -n 4 file.fastq

# Count sequences
grep "^>" file.fasta | wc -l

# Get statistics
samtools flagstat file.bam

# Extract region
samtools view file.bam chr1:1000-2000
```

---

## Key Takeaways

1. **FASTA**: Simple sequences, no quality → Use for reference genomes, databases
2. **FASTQ**: Sequences + quality scores → Use for raw sequencing data
3. **SAM**: Text alignment format → Use for viewing/converting alignments
4. **BAM**: Compressed binary alignment → Use for storage/fast processing
5. **VCF**: Variants only → Use for SNP/indel data
6. **Q-value calculation**: `Q = ASCII - 33` (Sanger encoding)
7. **Coordinates**: Most use 1-based (except BED = 0-based)
8. **Compression**: gz is standard for FASTQ, VCF; BAM is inherently compressed

---

## Quick Reference: ASCII to Phred Conversion

```python
# Python function to convert quality character to Q value
def ascii_to_q(char):
    return ord(char) - 33

# Examples
print(ascii_to_q('I'))  # 73 - 33 = 40 (excellent)
print(ascii_to_q('5'))  # 53 - 33 = 20 (acceptable)
print(ascii_to_q('#'))  # 35 - 33 = 2  (poor)
```

---

**Last Updated**: 2024 | **Standard**: SAM/BAM/VCF v4.2 | **Encoding**: Sanger/Illumina 1.8+