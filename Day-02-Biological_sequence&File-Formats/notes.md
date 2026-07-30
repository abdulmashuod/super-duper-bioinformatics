# Day 2 Learning Notes: Biological Sequences & File Formats
## 5-Minute Quick Read

---

## The Problem We're Solving

DNA sequencers produce millions of short DNA fragments (reads) every day. These reads need to be:
1. **Stored** in a standard format (not just random binary blobs)
2. **Shared** between labs and researchers globally
3. **Processed** by dozens of different software tools
4. **Tracked** with quality information about confidence

Without standardized file formats, every lab would invent their own format, making it impossible for tools to communicate. FASTA and FASTQ solved this problem.

---

## Timeline: Why These Formats Exist

```
1980s: FASTA created (simple, universal)
         ↓
2000s: DNA sequencers invented (need for quality info)
         ↓
2005: FASTQ created (FASTA + quality scores)
         ↓
2008: SAM format published (alignments)
         ↓
2009: BAM format (compressed SAM)
         ↓
2011: VCF format (variants standardized)
```

---

## Quick Concept: What Does Each Format Store?

### The Sequence Hierarchy

```
Raw sequencing data (millions of pieces)
        ↓ (stored as)
      FASTQ files
        ↓
    (software aligns to reference)
        ↓ (output as)
      SAM/BAM files (where does each read belong?)
        ↓
    (software calls variants)
        ↓ (output as)
      VCF files (what changed from reference?)
```

---

## Understanding FASTA (2 minutes)

### What It Looks Like
```
>sequence_name
ATGCATGCATGCATGC
ATGCATGCATGCATGC
```

### Why It's Simple
- **Line 1**: Starts with `>`, followed by sequence name
- **Lines 2+**: Just the bases (ATCG for DNA, AUGC for RNA)
- That's it. No metadata, no quality, no extra info.

### When to Use FASTA
-  Reference genomes (human genome from NCBI)
-  Protein databases (looking up amino acid sequences)
-  Sequence searching (BLAST searches)
-  When you DON'T care about quality info

### Real-World Example
```
>NM_001005484 Homo sapiens hemoglobin beta (HBB)
CCCTTGCTCCTCTCCTAGGACACCCAAGGTCAGGAGGCCAATATTTTAGGTACTGGTTAGTATTGGATGTGAGTGCTGACTGACTACCTGAGAGCTGTGGAGTTCAGGGTGAGG
```

---

## Understanding FASTQ (2 minutes)

### What It Looks Like
```
@read_name
ATGCATGCATGCATGC
+
IIIIIIIIHHHHHH##
```

### The 4-Line Pattern (You MUST Know This!)
```
Line 1: @name       (starts with @)
Line 2: sequence    (ATCG bases)
Line 3: +separator  (just a + sign)
Line 4: qualities   (ASCII characters, one per base)
```

### Why Line 4 Matters (The Phred Quality Score)

When a DNA sequencer reads a base, it's not 100% certain. For example:
- "I'm 99.9% sure this is an A" → quality score Q30
- "I'm 90% sure this is a G" → quality score Q10
- "I'm 63% sure this is a T" → quality score Q2

The quality line stores this confidence for each base using a **single ASCII character**.

### Converting ASCII Characters to Confidence Levels

**The magic formula:**
```
Q_value = ASCII_code - 33
```

#### Common Quality Characters
```
Character:  I   H   G   E   5   #
ASCII:      73  72  71  69  53  35
Q_value:    40  39  38  36  20  2
Meaning:    🟢  🟢  🟢  🟢  🟡  🔴
          99.99% 99.97% 99.84% 99.75% 99% 63%
```

**Reading Quality Scores:**
```
IIIIIIIII##    → Starts great, ends terrible
##########    → Entire read is garbage
IIIIIIIIII    → Perfect read, use it
HHHHHHHHH     → Very good read, use it
```

### When to Use FASTQ
- Raw sequencing data (fresh from Illumina machine)
- When you need quality information for filtering
- Before sequence assembly (quality guides algorithm)
- For variant calling (poor quality bases affect accuracy)

### Real-World Example
```
@SRR001666.1 071112_SOLEXADEV_s_1
GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTAAATCCATTTGTTCAACTCACAGTGT
+
!''*((((***+))%%%++)(%%%%).1***-+*''))**55CCF>>>>>>CCCCCCC65
```

**Quality Analysis:**
- Starts with `!` and `'` → Low quality bases at beginning
- Middle has many `F` and `>` → Excellent quality in middle
- Ends with `65` → Still decent at end
- **Action:** Trim first 10-15 bases, keep the rest

---

## Understanding SAM/BAM (Understanding at 30,000 feet)

### What Problem Do They Solve?

After sequencing, you have:
- Millions of reads (FASTQ)
- One reference genome (FASTA)

Question: **"Which chromosome does this read belong to?"**

Answer: SAM/BAM files

### SAM = Simple Text Version
```
read_001  163  chr1  1000  60  100M  =  1500  600  ATGCATGC...  IIIIIIII...
read_002  83   chr2  5000  45  95M   =  5100  500  GCTAGCTA...  HHHHHHHH...
```

### BAM = Compressed Binary Version
- Same info as SAM
- 10-20x smaller file
- Much faster to search
- Requires special tools to read (samtools)

### When to Use
- ✅ After alignment (mapping reads to genome)
- ✅ Finding where each read came from
- ✅ Detecting variants
- ✅ Coverage analysis

---

## Understanding VCF (Understanding at 30,000 feet)

### What Problem Do They Solve?

After aligning reads and calling variants, you ask:
**"What's different between this sample and the reference genome?"**

Answer: VCF files

### What It Contains
```
chr1  14370  rs6054257  G  A  29  PASS  DP=14;AF=0.5  GT:GQ  0/1:48  0/0:49
```

Breaking it down:
- **chr1 14370** = Position on chromosome 1
- **G → A** = Changed from G (reference) to A (sample)
- **PASS** = Looks reliable
- **0/1** = Heterozygous (one copy has change, one doesn't)
- **0/0** = Homozygous reference (no change)

### When to Use
- ✅ Storing discovered variants (SNPs, insertions, deletions)
- ✅ Comparing multiple samples
- ✅ Clinical variant interpretation
- ✅ Cancer mutation analysis

---

## Quick Decision Tree: Which Format Should I Use?

```
"I have raw sequencing data from the machine"
  └─→ Use FASTQ ✓
  
"I want to look up a reference genome"
  └─→ Use FASTA ✓
  
"I have reads and need to map them to a genome"
  └─→ Output SAM/BAM ✓
  
"I found variants and want to store them"
  └─→ Use VCF ✓
  
"I want to share genes/exons locations"
  └─→ Use GFF/GTF ✓
  
"I want to mark peak regions"
  └─→ Use BED ✓
```

---

## The Quality Score Deep Dive (Why It Matters)

### Why Scientists Care About Quality

Imagine two reads:
```
Read A: ATGCATGC  with quality IIIIIIII (Q40 on all bases)
Read B: ATGCATGC  with quality ####### (Q2 on all bases)
```

They look identical! But:
- **Read A**: We're 99.99% confident
- **Read B**: We're only 63% confident

If you ignore quality:
```
Variant Caller sees: G → A substitution
Without Quality:     "I'll call this as a variant"
With Quality:        "Wait, this base has Q2? Ignore it. Not a real variant."
```

**This is why FASTQ quality saves money and time** by preventing false variants.

### Common Quality Thresholds
```
Q ≥ 30  → "This is a high-quality base" (99.9% accurate)
Q 20-29 → "Borderline, use with caution"
Q < 20  → "Probably garbage, discard it"
```

---

## Hands-On Commands You Should Know

```bash
# View first read in FASTQ
head -4 file.fastq

# Count sequences
grep "^>" file.fasta | wc -l

# View first FASTA sequence
head -2 file.fasta

# Compress FASTQ (saves space)
gzip file.fastq  # Creates file.fastq.gz

# View compressed FASTQ without uncompressing
zcat file.fastq.gz | head -4

# Convert FASTQ to FASTA (remove quality)
seqtk seq -A input.fastq > output.fasta
```

---

## File Format Sizes (Reality Check)

```
Human genome FASTA:           3 GB
100x coverage FASTQ:          150 GB
100x coverage BAM (aligned):  15 GB
VCF of variants:              10 MB

Rule: FASTQ > SAM >> BAM, VCF
```

Why so different?
- FASTQ stores every base for millions of reads (HUGE)
- BAM compresses the same data (10-20x smaller)
- VCF only stores differences (tiny by comparison)

---

## Key Takeaways (Your Checklist)

By the end of this topic, you should understand:

- [ ] **FASTA structure**: `>header` + sequence lines
- [ ] **FASTQ structure**: 4-line pattern with quality scores
- [ ] **Q-value calculation**: ASCII - 33 = Q value
- [ ] **Quality interpretation**: Q30 = 99.9%, Q20 = 99%, Q10 = 90%
- [ ] **SAM/BAM purpose**: Stores alignments (where reads map)
- [ ] **VCF purpose**: Stores variants (what changed)
- [ ] **When to compress**: FASTQ is enormous, always use .gz
- [ ] **File conversions**: FASTQ → FASTA → SAM → BAM → VCF

---

## Common Beginner Mistakes to Avoid

❌ **Mistake 1:** Ignoring quality scores  
✅ **Fix:** Always check quality before analysis

❌ **Mistake 2:** Forgetting BED files are 0-based  
✅ **Fix:** VCF is 1-based, BED is 0-based (different!)

❌ **Mistake 3:** Trying to open BAM in a text editor  
✅ **Fix:** Use `samtools view` or specialized tools

❌ **Mistake 4:** Not compressing FASTQ files  
✅ **Fix:** Always use `.fastq.gz` for storage

❌ **Mistake 5:** Mixing up quality character interpretation  
✅ **Fix:** Use the ASCII-33 formula every time

---

## Next Steps (Prepare for Day 3)

Tomorrow: **NCBI Databases & Searching**

What you should know going in:
- Where do public sequences live? (NCBI!)
- How do I find sequences by gene name?
- How do I download sequences?

**Optional prep task:**
Visit https://www.ncbi.nlm.nih.gov/gene and search for "TP53 human". See how many different sequence formats are available for download. Tomorrow you'll understand all of them!

---

## Quick Reference Table

| Format | Content | Size | Compression | Main Use |
|--------|---------|------|-------------|----------|
| FASTA | Sequences only | S | No | Reference data |
| FASTQ | Sequences + Quality | XXL | Yes (.gz) | Raw reads |
| SAM | Alignments (text) | XXL | No | Viewing/Converting |
| BAM | Alignments (binary) | L | Built-in | Storage/Analysis |
| VCF | Variants | S | Yes (.gz) | Results reporting |

---

**Time to Read:** ~5 minutes | **Difficulty:** Beginner | **Last Updated:** 2024