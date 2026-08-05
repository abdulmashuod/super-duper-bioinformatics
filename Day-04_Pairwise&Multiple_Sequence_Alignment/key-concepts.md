# Day 4: Pairwise and Multiple Sequence Alignment - Comprehensive Guide

## Table of Contents
1. [Introduction to Sequence Alignment](#introduction-to-sequence-alignment)
2. [Fundamentals and Basics](#fundamentals-and-basics)
3. [Why Alignment Matters](#why-alignment-matters)
4. [Pairwise Sequence Alignment](#pairwise-sequence-alignment)
5. [Multiple Sequence Alignment](#multiple-sequence-alignment)
6. [Scoring Systems](#scoring-systems)
7. [Practical Applications](#practical-applications)
8. [Tools and Software](#tools-and-software)
9. [Interpretation and Analysis](#interpretation-and-analysis)

---

## Introduction to Sequence Alignment

### What is Sequence Alignment?

**Sequence alignment** is the process of arranging two or more biological sequences (DNA, RNA, or protein) to identify regions of similarity that may indicate functional, structural, or evolutionary relationships. It's one of the most fundamental operations in bioinformatics.

### The Core Concept

At its simplest, alignment answers the question: **"Which parts of these sequences are similar, and how did they diverge?"**

```
Without alignment:
Sequence 1: ATGCGATCG
Sequence 2: ATGCGATAG
Unclear which positions match

With alignment:
Sequence 1: ATGCGATCG
Sequence 2: ATGCGATAG
            |||||| *|
Clearly shows: 7 matches, 1 mismatch
```

### Why Alignment is Fundamental

Sequence alignment is the foundation for:
- **Homology detection**: Finding related sequences
- **Evolutionary inference**: Understanding how species diverged
- **Functional annotation**: Predicting gene/protein function
- **Disease studies**: Identifying disease-causing mutations
- **Drug development**: Finding therapeutic targets
- **Quality control**: Verifying sequence integrity

---

## Fundamentals and Basics

### Basic Principles

#### 1. **Sequence Similarity vs. Identity**

**Similarity**: Sequences share the same character at corresponding positions
- Used primarily for proteins (consider chemical similarity)
- Example: Leucine (L) and Isoleucine (I) are similar (both hydrophobic)

**Identity**: Sequences have exactly the same character at positions
- Used for any sequence type
- Example: A matches A, but A does not match G

**Why This Matters:**
```
DNA:        ATGC vs ATGC = 100% identity
Protein:    LILV vs LIVL = 0% identity, but high similarity
            (All are hydrophobic - chemically similar)
```

#### 2. **Alignment Operations**

Every alignment involves three types of operations:

| Operation | Symbol | Meaning | Example |
|-----------|--------|---------|---------|
| **Match** | \| | Identical characters | A matches A |
| **Mismatch** | * | Different characters | A mismatches T |
| **Insertion** | - | Gap in sequence 1 | -|T (inserted T) |
| **Deletion** | - | Gap in sequence 2 | A|- (deleted A) |

```
Alignment example:
Sequence 1: ATG-CGATCG
Sequence 2: ATGTCGATAG
            ||| |||| *|
Matches:    ///  ////  /
            Matches, mismatch at end
```

#### 3. **Gaps and Indels**

**Indel** = Insertion or Deletion (a gap in the sequence)

```
Sequence 1: ATGCGATCG
Sequence 2: ATGC---CG  (gap in sequence 2 represents deletion)

Alternative alignment:
Sequence 1: ATGC---GATCG
Sequence 2: ATGCGATCG--  (gap in sequence 1 represents insertion)

These different alignments have different biological meanings!
```

### Biological Basis of Alignment

#### Evolution Creates Sequence Variation

```
Ancestor: ATGCGATCG
    |
    ├─→ Organism A: ATGCGATCG (no change)
    |
    ├─→ Organism B: ATGCGATAG (point mutation: C→A)
    |
    └─→ Organism C: ATG--ATCG (deletion of CG)
```

All three sequences descended from a common ancestor. Alignment reveals this relationship.

#### Scoring Principles

An alignment is "good" if it:
1. **Maximizes similarity** at matched positions
2. **Minimizes total gaps** (gaps are biologically rare)
3. **Keeps gaps together** (indels tend to occur in clusters)
4. **Makes biological sense** (considers evolutionary plausibility)

---

## Why Alignment Matters

### Problem 1: The Information Gap

**Raw sequences tell us very little:**
```
Sequence A: MKVLSLLTCLWSSMACGPPQFVDVQSVDWLQEMKDVNDVCGMAA
Sequence B: MLVVLSLLTCLWSSMACGPPFVDVQSVDWLQEMKDVNDVCGAA
```

**Are these related?** Unknown.
**Do they have the same function?** Unknown.
**When did they diverge?** Unknown.

**Alignment reveals everything:**
```
Sequence A: M-KVLSLLTCLWSSMACGPPQFVDVQSVDWLQEMKDVNDVCGMAA
Sequence B: MLVVLSLLTCLWSSMACGPPFVDVQSVDWLQEMKDVNDVCGAA
            * **|||||||||||||||||* ||||||||||||||||||||||
            2 differences in 45 positions = 95.6% similarity
            → Highly conserved → Likely same function → Orthologous proteins
```

### Problem 2: Positional Equivalence

Different sequences may be different lengths, making direct comparison impossible.

```
Gene A: ATGCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
Gene B: ATGCGATCGATCGATCG

Which positions should be compared?
Without alignment: unclear
With alignment: position-by-position correspondence established
```

### Problem 3: Hidden Homology

Similar sequences might not look similar at first glance due to gaps and variations.

```
Protein A: MVLHYTNQK---LKPPNRT
Protein B: MVLHYTNQKPFPLKPPNRT

Without alignment: hard to see they differ by only 3 amino acids
With alignment: Clear that only one insertion separates them
```

### Why We Need Alignment: Core Applications

#### 1. **Homology Detection**
- Identify sequences from different organisms with common ancestry
- Enables functional transfer: if sequences are homologous, likely same function

#### 2. **Evolutionary Analysis**
- Calculate sequence divergence
- Infer phylogenetic relationships
- Estimate divergence time between species

#### 3. **Functional Prediction**
- Conserved regions often indicate functional importance
- Can predict function of uncharacterized sequence based on similar characterized one

#### 4. **Disease Mechanism Elucidation**
- Compare normal vs. disease-causing sequences
- Identify specific mutations responsible for disease
- Predict mutation impact

#### 5. **Protein Structure Prediction**
- Conserved residues often indicate structural importance
- Multiple alignment shows which positions are critical

#### 6. **Motif Discovery**
- Identify conserved patterns/motifs
- Indicates functional domains or binding sites

#### 7. **Quality Control**
- Verify sequence correctness by comparing to reference
- Identify contamination or sequencing errors

---

## Pairwise Sequence Alignment

### Overview: Comparing Two Sequences

Pairwise alignment is the simplest form: comparing exactly two sequences to find their optimal alignment.

### Types of Pairwise Alignment

#### 1. Global Alignment

**Definition**: Aligns entire sequences from start to end.

**Best for**: 
- Sequences of similar length
- Finding overall relationship
- Complete gene/protein comparison

**Philosophy**: "These sequences should align completely"

```
Example with similar-length sequences:
Sequence 1: ATGCGATCGA
Sequence 2: ATGCGATAGA
Alignment:  ATGCGATCGA
            ATGCGATAGA
            ||||||| *|
```

#### 2. Local Alignment

**Definition**: Finds regions of high similarity within sequences, ignoring divergent regions.

**Best for**:
- Sequences of different lengths
- Sequences with only partial similarity
- Finding conserved domains
- Comparing sequences with insertions/deletions

**Philosophy**: "These sequences share similar regions, even if overall different"

```
Example with partial similarity:
Sequence 1: ATGCGATCGATGATGATGATG---
Sequence 2: -------GATGATGATGATGCAG
Local alignment identifies the similar region:
         GATGATGATGATG
         GATGATGATGATG
         |||||||||||||
```

### Algorithm 1: Global Alignment (Needleman-Wunsch)

#### Historical Context
- Developed: 1970 by Saul Needleman and Christian Wunsch
- Revolutionary: First formal algorithm for sequence alignment
- Impact: Won "best paper of the decade" award

#### Core Principle: Dynamic Programming

Dynamic programming is an algorithmic technique where complex problems are solved by:
1. Breaking into smaller subproblems
2. Solving each subproblem once
3. Storing results to avoid recalculation
4. Building up to complete solution

#### Step-by-Step Walkthrough

**Input Sequences:**
```
Sequence 1 (horizontal): GCATGC
Sequence 2 (vertical):   GATTACA
```

**Step 1: Initialize Scoring Matrix**

Create matrix with:
- Rows = length of sequence 2 + 1
- Columns = length of sequence 1 + 1
- Initialize first row and column with gap penalties

```
        ""  G   C   A   T   G   C
    ""   0  -1  -2  -3  -4  -5  -6
    G   -1   
    A   -2
    T   -3
    T   -4
    A   -5
    C   -6
    A   -7
```

**Scoring Parameters:**
- Match score: +1 (reward for identical characters)
- Mismatch score: 0 (no penalty, just no reward)
- Gap penalty: -1 (penalty for inserting a gap)

**Step 2: Fill Matrix with Recurrence Relation**

For each cell [i,j], calculate:

```
Score[i,j] = max(
    Score[i-1,j-1] + match_score (if sequences match)
                   + mismatch_score (if they don't),
    Score[i-1,j] + gap_penalty,    (deletion)
    Score[i,j-1] + gap_penalty     (insertion)
)
```

**Filling the matrix (showing key calculations):**

```
        ""  G   C   A   T   G   C
    ""   0  -1  -2  -3  -4  -5  -6
    G   -1   1   0  -1  -2  -3  -4
    A   -2   0   1   1   0  -1  -2
    T   -3  -1   0   1   2   1   0
    T   -4  -2  -1   0   2   2   1
    A   -5  -3  -2   0   1   2   2
    C   -6  -4  -2  -1   1   2   3
    A   -7  -5  -3  -1   0   1   2
```

**Explanation of filled cells:**

```
Cell [1,1] (G vs G):
- Diagonal [0,0] + match (+1) = 0 + 1 = 1
- Up [0,1] + gap (-1) = -1 - 1 = -2
- Left [1,0] + gap (-1) = -1 - 1 = -2
Maximum = 1 ✓ (match is best)

Cell [2,3] (G-A, G-A-T):
- Diagonal [1,2] + mismatch (0) = 1 + 0 = 1
- Up [1,3] + gap (-1) = -1 - 1 = -2
- Left [2,2] + gap (-1) = 1 - 1 = 0
Maximum = 1 ✓
```

**Step 3: Traceback to Find Alignment**

Starting from bottom-right cell [7,6] = 2, trace back to [0,0]:
- If came from diagonal: match/mismatch
- If came from up: deletion
- If came from left: insertion

```
Traceback path: [7,6] → [6,5] → [5,4] → [4,3] → [3,2] → [2,1] → [1,1] → [0,0]

Converting path to alignment:
- [6,5]→[7,6]: Diagonal (A-A match): A aligns with A
- [5,4]→[6,5]: Diagonal (A-C mismatch): A aligns with C
- [4,3]→[5,4]: Diagonal (T-T match): T aligns with T
- [3,2]→[4,3]: Diagonal (T-A mismatch): T aligns with A
- [2,1]→[3,2]: Diagonal (A-G mismatch): A aligns with G
- [1,1]→[2,1]: Up (G-gap): G aligns with gap
- [0,0]→[1,1]: Diagonal (G-G match): G aligns with G
```

**Final Alignment:**
```
Sequence 1: G-CATGC
Sequence 2: GATTACA
            *|* * *
Score: 2
```

#### Output Interpretation

- **Alignment score: 2** (relatively low, indicates sequences not very similar)
- **Alignment**: Shows which positions match/mismatch
- **Aligned length**: 7 (includes gaps)
- **Identity**: 3/7 = 42.9%

#### When to Use Needleman-Wunsch

✓ Similar length sequences
✓ Want complete alignment
✓ Interested in overall relationship
✓ Sequences expected to be similar across entire length

✗ Very different length sequences
✗ Only interested in local similarity
✗ High indel rate

---

### Algorithm 2: Local Alignment (Smith-Waterman)

#### Historical Context
- Developed: 1981 by Temple Smith and Michael Waterman
- Improvement: Overcame limitations of global alignment
- Key innovation: Can find similar regions without requiring entire sequence alignment

#### Core Principle

Smith-Waterman uses dynamic programming like Needleman-Wunsch, but with **two key differences**:

1. **Recurrence relation allows zero** (can start fresh alignment)
2. **Start traceback from highest score** (not necessarily corner)

#### Recurrence Relation

```
Score[i,j] = max(
    Score[i-1,j-1] + match/mismatch_score,
    Score[i-1,j] + gap_penalty,
    Score[i,j-1] + gap_penalty,
    0  ← KEY DIFFERENCE: Can start fresh!
)
```

#### Step-by-Step Walkthrough

**Input Sequences:**
```
Sequence 1: AGTACGCA
Sequence 2: TATGCGCA
```

**Observation**: Sequences have a conserved region but differ at ends.
Goal: Find the best local alignment (conserved region).

**Initialize and Fill Matrix:**

```
        ""  A   G   T   A   C   G   C   A
    ""   0   0   0   0   0   0   0   0   0
    T    0   0   0   1   0   0   0   0   0
    A    0   1   0   0   1   0   0   0   1
    T    0   0   0   1   0   0   0   0   0
    G    0   0   1   0   0   0   1   0   0
    C    0   0   0   0   0   1   0   2   0
    G    0   0   0   0   0   0   1   1   1
    C    0   0   0   0   0   1   0   2   1
    A    0   1   0   0   1   0   0   1   3
```

**Key Calculation (Cell [5,6] - C vs G):**
```
Above: [4,6] = 1, gap penalty -2, so 1-2 = -1 → 0 (can use zero)
Left: [5,5] = 1, gap penalty -2, so 1-2 = -1 → 0 (can use zero)
Diagonal: [4,5] = 1, mismatch (C≠G) score 0, so 1+0 = 1
Result: max(0, 0, 1, 0) = 1
```

**Step 2: Find Starting Point for Traceback**

Search matrix for **highest score** (not necessarily in corner):
```
Maximum score = 3 at position [8,8] (A vs A)
```

**Step 3: Traceback from [8,8] until Score Reaches 0**

```
[8,8]: A-A match (score 3)
[7,7]: C-C match (score 2)
[6,6]: G-G match (score 1)
[5,5]: C-C match (score 1)
[4,4]: A-A match (score 1)
Stop when score would go to 0
```

**Result: Local Alignment**

```
Sequence 1: TACGCA
Sequence 2: TACGCA
            ||||||
Alignment score: 6 (perfect match!)
Identity: 100%
```

**Full Sequences vs Local Alignment:**
```
Sequence 1: AGTACGCA
Sequence 2: TATGCGCA
            ^^│││││^
            ││ conserved region found ││
```

#### When to Use Smith-Waterman

✓ Sequences of different lengths
✓ Only interested in similar regions
✓ Sequences with variable similarity
✓ Finding conserved domains
✓ No prior knowledge of homology

✗ Want complete global picture
✗ Expect overall similarity

---

### Practical Pairwise Alignment Examples

#### Example 1: Highly Similar Sequences (>95% identity)

```
Sequence 1: ATGCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
Sequence 2: ATGCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
Alignment:  ATGCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
            ||||||||||||||||||||||||||||||||||||||||
Identity: 100%
Conclusion: Same sequence, likely same gene from same species
```

**What it tells us**: No mutations. Sequences are identical (or same sample sequenced twice).

#### Example 2: Moderately Similar Sequences (80-95% identity)

```
Sequence 1: ATGCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
Sequence 2: ATGCGATCGATCGATCGATCGTTCGATCGATCGATCGATCG
            ||||||||||||||||||||||*|||||||||||||||||
            40 matches, 1 mismatch (C→T at position 25)
Identity: 97.5%
Conclusion: Allelic variants or sequences from different strains of same species
```

**What it tells us**: Point mutation present. Likely same gene with natural variation.

#### Example 3: Sequences with Insertions/Deletions

```
Sequence 1: ATGCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
Sequence 2: ATGCGAT--ATCGATCGATCGATCGATCGATCGATCGATCG
            ||||||| XX||||||||||||||||||||||||||||||||
            Deletion of 2 bases (CG) at position 8
Identity: 95.1% (accounting for gap)
Conclusion: Allelic variant or mutation in different organism
```

**What it tells us**: Indel event occurred. Biological significance depends on position.

#### Example 4: Sequences from Different Species (50-80% identity)

```
Sequence 1: ATGCGATCGATCGATCGATCGATCGATCGATCGATCGATCG (Human)
Sequence 2: ATGCGATTGATCGATCGTTGATCGATCGATCGATCGATTG (Mouse)
            |||||||*||||||||||*||||||||||||||||||| *||
            37 matches, 3 mismatches
Identity: 90%
Conclusion: Orthologous sequences from different species
```

**What it tells us**: Sequences are related through speciation. Function likely conserved.

#### Example 5: Distantly Related Sequences (<50% identity)

```
Sequence 1: ATGCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
Sequence 2: ATGAGATCGATCGATCGATCGACTGATCGATCGATTGATCG
            ||*|||||||||||||||||||||*||||||||||*|||||
            37 matches, 3 mismatches
But when checking with significance test: NOT statistically significant
Conclusion: May not be related - need further investigation
```

**What it tells us**: Similarity might be by chance. Homology questionable.

---

## Multiple Sequence Alignment

### Overview: Aligning More Than Two Sequences

**Multiple Sequence Alignment (MSA)** extends pairwise concepts to align 3 or more sequences simultaneously, revealing patterns and relationships across a family of sequences.

### Why MSA is Different from Pairwise

**Pairwise**: One-to-one comparison
```
Seq A vs Seq B: Find best matching positions
```

**Multiple**: One-to-many-to-many comparison
```
Seq A vs Seq B vs Seq C vs Seq D: Find positions that match across ALL sequences
```

**Challenge**: Can't use simple dynamic programming (computationally infeasible for many sequences)

### Types of MSA Approaches

#### 1. **Progressive Alignment (Most Common)**

**Strategy**: Build MSA iteratively from pairwise comparisons

**Steps:**
1. Calculate pairwise similarities between all sequences
2. Build a guide tree showing relationships
3. Align most similar pairs first
4. Progressively add more divergent sequences

**Advantage**: Computationally efficient
**Disadvantage**: Early errors propagate to later stages

```
Sequences:     A, B, C, D
               ↓
Pairwise comparison: A↔B (99%), A↔C (85%), A↔D (60%)
               ↓
Guide tree:
        ┌─────A
        │
    ┌───┤     ┌─B
    │   └─────┤
    │         └─C
    │
    └─────D

Alignment order: A+B → (AB)+C → (ABC)+D
```

#### 2. **Iterative Refinement**

**Strategy**: Improve alignment through repeated optimization

**Process:**
1. Create initial MSA using progressive method
2. Remove one sequence at a time
3. Realign removed sequence to others
4. Repeat until convergence

**Advantage**: Can correct early errors
**Disadvantage**: Computationally expensive

#### 3. **Consistency-Based Methods**

**Strategy**: Consider consistency across pairwise alignments

**Idea**: If A-B alignment suggests position X matches Y, and B-C alignment confirms, then in A-C alignment, X and Y should also match.

**Advantage**: More reliable
**Disadvantage**: Complex calculations

### Step-by-Step MSA Example

**Input Sequences:**
```
Sequence A: GCATGCG
Sequence B: GATTACA
Sequence C: GCATACA
```

**Step 1: Calculate Pairwise Similarities**

```
A vs B: 4 matches in 7 positions = 57% identity
A vs C: 6 matches in 7 positions = 86% identity
B vs C: 5 matches in 7 positions = 71% identity

Most similar: A-C (86%)
Least similar: A-B (57%)
```

**Step 2: Build Guide Tree**

```
        ┌──C (86% similar to A)
    ┌───┤
────┤   └──A
    │
    └──────B (most divergent)
```

**Step 3: Align Most Similar Pair First (A+C)**

```
Sequence A: GCATGCG
Sequence C: GCATACA
Alignment:  GCATGCG
            GCATACA
            |||||*|
            6 matches, 1 mismatch at position 5 (G≠A)
```

**Step 4: Add Third Sequence (B)**

Align B to the (A-C) alignment profile:

```
Current alignment profile:
            GCATGCG
            GCATACA

Aligning B:
GATTACA
G A - T - T A C A
││ │ │ │ │ │ │ │
│└─match at position 1 (G)
└─match at position 1 (G)
```

**Result:**

```
Position:   1 2 3 4 5 6 7
Sequence A: G C A T G C G
Sequence C: G C A T A C A
Sequence B: G A - T T - A

MSA:        G  C  A  T  ?  C  A
            │  │  │  │  ?  │  │
            A  B  A  B  C  A  B

Position 5:
- A has G
- B has T
- C has A
→ Multiple mutations, or not all aligned (ambiguous position)
```

**Final Multiple Alignment:**
```
Sequence A: G C A T G C G
Sequence B: G A - T T - A
Sequence C: G C A T A C A
            │ │ * │ * * │
```

**Analysis:**
- **Highly conserved**: Position 1 (all G), Position 3 (all A), Position 7 (all A)
- **Variable**: Position 5 (G, T, A)
- **Insertions/Deletions**: Position 2 in B, Position 6 in B

**Biological interpretation:**
- Positions 1, 3, 7 are critical (conserved across evolution)
- Positions 5, 6 can tolerate variation
- Position 2 in B has an insertion

---

### Common MSA Algorithms

#### 1. **ClustalW (Weighted Cluster Analysis)**

**Algorithm:**
- Progressive alignment with position weighting
- Each sequence weighted based on divergence
- More similar sequences have less weight (avoid overrepresentation)

**Advantages:**
- Well-established, widely used
- Fast enough for large datasets
- Works well for diverse sequence sets

**Process:**
```
1. Calculate distance matrix (all pairwise distances)
2. Build UPGMA tree (guide tree)
3. Progressively align using weights
4. Output MSA with quality scores
```

#### 2. **MUSCLE (Multiple Sequence Comparison by Log-Expectation)**

**Algorithm:**
- Three-stage process: draft → improve → refine
- More accurate than ClustalW

**Advantages:**
- High accuracy
- Fast even for many sequences
- Better gap placement than ClustalW

**Process:**
```
Draft MSA → Compute tree → Refine alignments → Output final MSA
```

#### 3. **MAFFT (Multiple Alignment using Fast Fourier Transform)**

**Algorithm:**
- Uses FFT to identify similar regions
- Very fast, high accuracy

**Advantages:**
- Fastest for large datasets
- Highly accurate
- Handles sequences of very different lengths

**Process:**
```
1. Group similar sequences
2. FFT-based similarity calculation
3. Iterative alignment
4. Refinement
```

#### 4. **T-Coffee (Tree-based Consistency Objective Function for Alignment Evaluation)**

**Algorithm:**
- Consistency-based approach
- Considers all pairwise alignments simultaneously

**Advantages:**
- Most accurate (consistency-based)
- Good for divergent sequences

**Disadvantage:**
- Computationally expensive for large datasets

---

### Advanced MSA Topics

#### Profile Alignment

**Concept**: Instead of comparing individual sequences, compare groups of sequences

```
Group 1:    Group 2:
Seq A       Seq C
Seq B       Seq D

Profile1:   Profile2:
Consensus:  GC-AT   Consensus: GCATA
Freq. at 1: G(100%) Freq. at 1: G(100%)
Freq. at 2: C(100%) Freq. at 2: C(100%)

Align profiles (summarized sequences) instead of individual sequences
→ More efficient and more reliable
```

#### Hidden Markov Models (HMMs)

**Concept**: Probabilistic model of sequence variation

**Idea:**
- At each position, define probability distribution of amino acids
- Position with high conservation has skewed distribution (one amino acid dominant)
- Position with variation has flat distribution (many amino acids equally likely)

```
Position 1:       Position 5:
A: 95%            A: 25%
C:  3%            C: 25%
G:  2%            G: 25%
T:  0%            T: 25%
│                 │
Highly conserved  Highly variable
```

#### Motif Discovery

**Concept**: Find common patterns in MSA

```
MSA:
Seq1: ATGCGATCGATCGAT
Seq2: ATGCGATAGGTCGAT
Seq3: ATGCGATCGATCGAA
      ││││││││ ││││││

Motif: ATGCGAT (found in all sequences)
       This 7bp motif is conserved → Likely important
```

---

## Scoring Systems

### Scoring Matrices for Proteins

#### 1. **PAM (Point Accepted Mutation) Matrices**

**Historical Context:**
- Developed: Margaret Dayhoff (1978)
- PAM1 = 1% amino acid change per position

**Philosophy**: Based on observed point mutations in proteins

**Types:**
- PAM30: Short evolutionary distance (very similar sequences)
- PAM70: Medium distance
- PAM250: Long evolutionary distance (distant sequences)

**Interpretation:**
```
PAM1:  Sequences 99% identical (very recent divergence)
PAM30: Sequences ~80% identical (short time)
PAM70: Sequences ~62% identical (medium time)
PAM250: Sequences ~20% identical (ancient divergence)
```

**How to use:**
```
Query: Recent orthologs? → Use PAM30
Query: Distant paralogs? → Use PAM250
```

#### 2. **BLOSUM (Blocks Substitution Matrix)**

**Historical Context:**
- Developed: Steven Henikoff and Jorja Henikoff (1992)
- Based on conserved blocks of protein sequences
- More empirical than PAM

**Interpretation:**
```
BLOSUM90: 90% identity within blocks
          → Very similar sequences (recent divergence)

BLOSUM62: 62% identity within blocks
          → Moderate divergence (most common for general use)

BLOSUM45: 45% identity within blocks
          → Distant sequences (ancient divergence)

Rule: Higher number = more similar sequences expected
      Lower number = more divergent sequences expected
```

**Matrix Values:**
```
Example BLOSUM62 entries:
A-A:  4  (rewarding if A matches A)
A-C:  0  (neutral if A matches C)
C-W: -8  (penalizing if C matches W - very different)
```

**How it works:**
```
For each position in alignment:
- If residues match: +4 (if both A)
- If residues different but similar: +1 (e.g., L-I)
- If residues very different: -3 or more (e.g., R-P)

Total score = sum of all position scores
```

#### 3. **Nucleotide Scoring**

**Simple DNA/RNA Scoring:**
```
Match:      +5
Mismatch:   -4
Gap open:   -12
Gap extend: -1
```

**Rationale:**
- Mutations less frequent than in proteins
- Gaps less frequent in nucleotides
- Different penalties for opening vs. extending gaps

### Gap Penalties

#### Why Gap Penalties Matter

Without penalties, algorithm would just insert gaps everywhere:
```
Without penalties:        With penalties:
Seq A: ATGC              Seq A: ATGC
Seq B: --ATGC-           Seq B: ATGC
       (meaningless)      (meaningful)
```

#### Types of Gap Penalties

**1. Linear Gap Penalty**
```
Cost = gap_penalty × gap_length
Gap of length 3 at penalty -2: 3 × (-2) = -6
```

**Problem**: Doesn't distinguish between one large gap and three small gaps
- Biologically: one deletion event produces one gap (preferable)
- Actually: three separate mutations produce three gaps

**2. Affine Gap Penalty**
```
Cost = gap_open_penalty + (gap_extend_penalty × gap_length)

Gap of length 3:
= -12 (opening) + (-2 × 2) = -12 + (-4) = -16

This penalizes opening more than extending
→ Encourages larger single gaps over multiple small gaps
```

**Advantages**: Biologically realistic

#### Gap Penalty Selection

```
Query: Short sequences, very similar?
→ Small gap penalty (e.g., -2), encourage gapped alignment

Query: Long sequences, moderate similarity?
→ Moderate gap penalty (e.g., -8/-2 for open/extend)

Query: Ancient divergence?
→ Large gap penalty, expect few gaps
```

---

## Practical Applications

### Application 1: Gene Annotation

**Scenario**: New gene sequence identified in human genome

**Process:**
1. BLAST search against GenBank (pairwise alignment)
2. Find similar sequences from characterized genes
3. Align to known genes using local alignment
4. Identify conserved domains
5. Predict function based on homologous genes

**Outcome**: Annotate new gene with predicted function

### Application 2: Mutation Analysis in Disease

**Scenario**: Patient with genetic disease, sequence mutated gene

**Process:**
1. Align patient sequence to normal reference sequence (pairwise)
2. Identify exact mutations
3. Determine if missense (changes amino acid) or synonymous (no change)
4. Predict mutation impact on protein
5. Align to homologs (MSA) to see if position conserved
6. If position conserved: mutation likely pathogenic

**Outcome**: Determine mutation causality

### Application 3: Phylogenetic Analysis

**Scenario**: Compare genes from 10 different species

**Process:**
1. Obtain gene sequences from all species
2. Perform multiple sequence alignment
3. Calculate evolutionary distances from alignment
4. Build phylogenetic tree
5. Infer evolutionary history

**Outcome**: Understand evolutionary relationships

### Application 4: Protein Structure Prediction

**Scenario**: Protein of unknown structure

**Process:**
1. BLAST search to find similar proteins with known structures
2. Multiple alignment of homologous proteins
3. Identify conserved regions (likely structural)
4. Identify variable regions (likely flexible)
5. Use MSA as template for structure prediction

**Outcome**: Predict 3D structure

### Application 5: Primer Design

**Scenario**: Design PCR primers to amplify gene from multiple species

**Process:**
1. Obtain gene sequences from target species
2. Multiple sequence alignment
3. Identify conserved regions across species
4. Design primers in conserved regions
5. Ensure primers work across all species

**Outcome**: Successful multi-species PCR

### Application 6: Domain Identification

**Scenario**: Identify functional domains in new protein

**Process:**
1. Search databases for similar proteins (BLAST)
2. Multiple alignment of similar proteins
3. Identify conserved motifs/domains
4. Compare to known domain databases
5. Assign putative function to domains

**Outcome**: Functional annotation

---

## Tools and Software

### Web-Based Tools

#### NCBI BLAST (blastp, blastn)
- **Best for**: Quick similarity searches
- **Interface**: Web-based, user-friendly
- **Speed**: Fast (seconds to minutes)
- **Output**: Pairwise alignments with scores
- **URL**: https://blast.ncbi.nlm.nih.gov/

**Usage Example:**
```
1. Go to BLAST website
2. Enter sequence (FASTA format)
3. Select database (protein/nucleotide)
4. Run search
5. View alignments with expect values
```

#### ClustalW Web Server
- **Best for**: Quick MSA
- **Input**: Sequences in FASTA format
- **Output**: Aligned sequences + phylogenetic tree
- **Visualization**: Sequence coloring shows conservation

#### MUSCLE (Web Server)
- **Best for**: Faster, more accurate than ClustalW
- **Input**: Sequences in FASTA format
- **Output**: Aligned sequences

### Desktop Software

#### MEGA (Molecular Evolutionary Genetics Analysis)
- **Features**: Alignment + phylogenetics + statistics
- **Interface**: GUI, user-friendly
- **Alignment**: Built-in or external tools
- **Strength**: Integrated evolutionary analysis

#### Jalview
- **Purpose**: Visualize and analyze MSA
- **Features**: Color schemes, consensus, secondary structure
- **Input**: Aligned sequences
- **Strength**: Beautiful visualization, feature annotations

#### SeqBuilder/SeqMan
- **Commercial software**: Expensive but powerful
- **Features**: Complete sequence analysis pipeline
- **Strength**: Quality, comprehensive toolset

### Command-Line Tools (For Bioinformaticians)

#### BLAST (NCBI)
```bash
blastp -query seq.fasta -db protein_db -evalue 1e-10
```

#### MUSCLE
```bash
muscle -in sequences.fasta -out aligned.afa
```

#### ClustalW
```bash
clustalw2 -INFILE=sequences.fasta -ALIGNMENT
```

#### MAFFT
```bash
mafft sequences.fasta > aligned.fasta
```

---

## Interpretation and Analysis

### Reading Alignment Output

#### Alignment Statistics

```
Example output:
Alignment length:       247 bp/aa
Matches:               235 positions
Mismatches:            12 positions
Gaps:                  0
Percent Identity:      95.1%
```

**Interpretation:**
- 95.1% identity = highly similar sequences
- No gaps = similar length, no indels
- Likely same species or very recent divergence

#### Expected Value (E-value)

**Definition**: Probability that alignment occurred by chance

```
E-value: 1e-50  →  1 in 10^50 chance = HIGHLY SIGNIFICANT
E-value: 0.01   →  1 in 100 chance = Marginal
E-value: 10     →  1 in 10 chance = NOT significant
```

**Rule of Thumb:**
```
E-value < 0.001:    Probably homologous
E-value 0.001-0.1:  Possibly homologous (investigate further)
E-value > 0.1:      Likely NOT homologous
```

### Alignment Quality Assessment

#### Conservation Analysis

```
Alignment:
Seq A: MKVLSLLTCLWSSMACGPPQFVDVQSVDWLQEMKDVNDVCGMAA
Seq B: MLVVLSLLTCLWSSMACGPPFVDVQSVDWLQEMKDVNDVCGAA
       **     ***|||||||||  ||||||||||||||||||||||||
       ││     
       High variation (likely surface)
                ││││││────────────────────
                Highly conserved (likely core/active site)
```

**Insights:**
- Conserved regions likely functionally important
- Variable regions likely structurally flexible/surface-exposed

#### Biological Significance

```
High percent identity (>90%): Likely same function
Moderate identity (50-90%): Likely same function (conserved domains)
Low identity (<50%): Questionable (check E-values)
```

### Common Interpretation Mistakes

**Mistake 1**: High % identity = definitely homologous
```
Problem: Can be high by chance for short sequences
Solution: Check E-value, not just % identity
```

**Mistake 2**: Gaps always indicate insertions/deletions
```
Problem: Gaps can be artifacts (alignment errors)
Solution: Check gap positions, verify with other alignments
```

**Mistake 3**: All conserved positions are functionally important
```
Problem: Some conservation due to structural constraints
Solution: Compare multiple orthologs to find critical positions
```

**Mistake 4**: Alignment score comparable across different sequences
```
Problem: Scores depend on length, composition, divergence
Solution: Use E-values or normalized scores for comparison
```

---

## Best Practices and Guidelines

### Before Alignment

**Checklist:**
- [ ] Sequences verified for accuracy
- [ ] Removed obvious sequencing artifacts
- [ ] Sequences in proper format (FASTA)
- [ ] Checked for contamination or mixed sequences
- [ ] Noted sequence source and metadata

### Choosing Alignment Type

**Use Global Alignment If:**
- Sequences similar length
- Expect full-length homology
- Comparing genes/proteins from same organism

**Use Local Alignment If:**
- Sequences different length
- Only partial similarity expected
- Looking for conserved domains
- Comparing across distant species

### Choosing MSA Algorithm

**Use ClustalW If:**
- Quick analysis needed
- Moderate number of sequences
- Consistent quality desired

**Use MUSCLE If:**
- Want better accuracy than ClustalW
- Large number of sequences
- Good balance of speed/quality

**Use MAFFT If:**
- Very large dataset (100+ sequences)
- Speed critical
- Sequences very different lengths

**Use T-Coffee If:**
- Highest accuracy needed
- Small-medium dataset
- Time not critical

### Parameter Selection

**Gap Penalties:**
```
Default: Open -12, Extend -2 (good for most cases)
Conservative: Open -20, Extend -4 (fewer, longer gaps)
Liberal: Open -5, Extend -1 (many, small gaps)
```

**Scoring Matrix:**
```
DNA: Simple match/mismatch
Protein (recent divergence): BLOSUM90, PAM30
Protein (moderate): BLOSUM62, PAM70 (DEFAULT)
Protein (ancient): BLOSUM45, PAM250
```

### Validation

**Always verify MSA by:**
1. Visual inspection (use Jalview or similar)
2. Check if conserved positions make sense
3. Verify with multiple algorithms
4. Compare to structure (if available)
5. Check literature for known functional sites

### Documentation

**For reproducibility, record:**
```
Algorithm: ClustalW v2.1
Database: GenBank (accessed 2024-01-15)
Sequences: 15 proteins from Mammalia
Scoring matrix: BLOSUM62
Gap parameters: Open -12, Extend -2
Output format: FASTA
Result: 247 bp alignment, 93% average identity
```

---

## Summary of Key Concepts

### Pairwise Alignment
- **Global (Needleman-Wunsch)**: Full sequence comparison, optimal for similar length
- **Local (Smith-Waterman)**: Find conserved regions, optimal for different lengths
- **Dynamic programming**: Computational backbone for both

### Multiple Sequence Alignment
- **Progressive**: Build iteratively, fast
- **Iterative**: Refine initial alignment, accurate
- **Consistency-based**: Consider all pairwise comparisons

### Scoring Systems
- **Proteins**: PAM or BLOSUM (choose based on divergence)
- **DNA**: Simple match/mismatch with gap penalties
- **Gap penalties**: Distinguish opening vs. extending gaps

### Applications
- Gene annotation through homology
- Mutation pathogenicity prediction
- Phylogenetic analysis
- Structure prediction
- Functional domain identification

### Quality Indicators
- **E-value**: Statistical significance
- **Identity %**: Sequence similarity (interpret carefully)
- **Conservation patterns**: Functional importance indicators

---

## Additional Resources

### Key Papers
- Needleman & Wunsch (1970) - Global alignment
- Smith & Waterman (1981) - Local alignment
- Dayhoff et al. (1978) - PAM matrices
- Henikoff & Henikoff (1992) - BLOSUM matrices

### Online Tools
- NCBI BLAST: https://blast.ncbi.nlm.nih.gov/
- ClustalW: https://www.ebi.ac.uk/Tools/msa/clustalw2/
- MUSCLE: https://www.ebi.ac.uk/Tools/msa/muscle/
- MAFFT: https://mafft.cbrc.jp/alignment/server/

### Learning Resources
- NCBI BLAST Tutorial: https://www.ncbi.nlm.nih.gov/guide/
- Jalview Tutorial: http://www.jalview.org/
- Bioinformatics courses: Coursera, Udemy

### Reference Books
- "Introduction to Computational Biology" - Michael S. Waterman
- "Bioinformatics: Sequence and Genome Analysis" - David W. Mount
- "Biological Sequence Analysis" - Durbin et al.