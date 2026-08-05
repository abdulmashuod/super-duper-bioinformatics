# Day 4: Pairwise and Multiple Sequence Alignment - Quick Revision Notes

---

## Core Concept in One Line
**Alignment** = Finding corresponding positions in sequences to identify similarity, homology, and evolutionary relationships.

---

## The Why (Quick Reasons We Need Alignment)

| Problem | Solution |
|---------|----------|
| Raw sequences unclear | Alignment reveals homology |
| Can't compare easily | Alignment creates position correspondence |
| Hidden similarity | Alignment shows conserved regions |
| Mutations vs species differences? | Alignment distinguishes both |
| Gene function unknown? | Alignment enables functional transfer |
| Evolutionary relationship? | Alignment enables phylogenetics |

---

## Fundamental Concepts

### Similarity vs Identity

| Term | Definition | Example |
|------|-----------|---------|
| **Identity** | Exact same character | A matches A (100%), A vs T (0%) |
| **Similarity** | Similar amino acids | L and I both hydrophobic (similar) |
| **Conservative substitution** | Change to similar AA | K→R (both positive charge) |
| **Non-conservative** | Change to different AA | K→A (positive→nonpolar) |

### Three Alignment Operations

```
Match:     Seq1: A   Seq2: A    Result: | (identical)
           
Mismatch:  Seq1: A   Seq2: T    Result: * (different)
           
Gap:       Seq1: A   Seq2: -    Result: - (insertion/deletion)
                            OR
           Seq1: -   Seq2: T    Result: - (insertion/deletion)
```

### Indel (Insertion/Deletion)

```
Original:   ATGCGATCG
With gap:   ATGC---CG  (CG deleted = 3bp indel)
Or:         ATG---GATCG (CG-T inserted)

Key: Different alignments have DIFFERENT meanings
     But both explain the sequence difference
```

---

## PAIRWISE ALIGNMENT (Comparing 2 Sequences)

### Quick Decision Tree: Which Algorithm?

```
                    START
                      ↓
          Similar length sequences?
          /                      \
        YES                      NO
        ↓                         ↓
    Want full              Want local
    alignment?             regions?
    ↓                       ↓
  GLOBAL              Use LOCAL
(Needleman-Wunsch)  (Smith-Waterman)
  NW Algorithm         SW Algorithm
```

---

## Algorithm 1: Global Alignment (Needleman-Wunsch)

### Overview
- **Goal**: Align ENTIRE sequences
- **Method**: Dynamic programming
- **Best for**: Similar length, expect full homology
- **Avoid**: Different length sequences

### Algorithm Steps Summary

**Step 1: Create Matrix**
```
Size: (Seq1_length + 1) × (Seq2_length + 1)
Initialize: First row/column = cumulative gap penalties
```

**Step 2: Fill Each Cell**
```
For each cell [i,j], calculate:
Score = MAX(
    diagonal + match/mismatch,
    up + gap_penalty,
    left + gap_penalty
)
```

**Step 3: Traceback**
```
Start from bottom-right [n,m]
Move backwards to [0,0]
Record path (diagonal/up/left)
Convert path to alignment
```

### Quick Example: GCATGC vs GATTACA

**Process:**
```
Create 8×7 matrix
Fill with scores (match +1, mismatch 0, gap -1)
Traceback from [7,6] to [0,0]
Result: G-CATGC / GATTACA  (Score: 2)
```

**Key Insight**: Every cell depends on previously calculated cells (dynamic programming magic)

### Recurrence Formula
```
dp[i][j] = max(
    dp[i-1][j-1] + score(seq1[i], seq2[j]),   // diagonal
    dp[i-1][j] - gap_penalty,                   // up (deletion)
    dp[i][j-1] - gap_penalty                    // left (insertion)
)
```

### When to Use
✓ Sequences similar length (like 100bp vs 110bp)
✓ Expecting overall homology
✓ Complete genomes comparison
✗ One sequence much longer
✗ Only interested in local match

---

## Algorithm 2: Local Alignment (Smith-Waterman)

### Overview
- **Goal**: Find BEST LOCAL regions (substrings)
- **Method**: Dynamic programming with reset
- **Best for**: Different lengths, partial similarity
- **Avoid**: When wanting complete alignment

### Key Difference from Global
```
Recurrence adds 0 (can start fresh):

dp[i][j] = max(
    dp[i-1][j-1] + score(...),
    dp[i-1][j] - gap_penalty,
    dp[i][j-1] - gap_penalty,
    0  ← THIS IS THE DIFFERENCE!
)
```

**Meaning**: If score becomes negative, restart from 0 (don't accumulate penalties)

### Algorithm Steps
**Step 1**: Create matrix and fill (like global)
**Step 2**: Find HIGHEST score in matrix (not in corner!)
**Step 3**: Traceback from highest score UNTIL hitting 0
**Step 4**: Convert path to alignment

### Quick Example: AGTACGCA vs TATGCGCA

**Key Points:**
- Sequences have different composition at ends
- Both have TACGC region in common
- Local alignment finds: TACGCA (perfect match)
- Ignores divergent regions

**When to Use**
✓ Very different length sequences
✓ Only interested in conserved domains
✓ Finding similar regions
✓ Sequences from distant species
✗ Want complete gene comparison

---

## SCORING SYSTEMS FOR ALIGNMENT

### Gap Penalties Explained

**Linear Penalty:**
```
Cost = penalty × gap_length
Gap length 3 = 3 × (-2) = -6
Problem: Doesn't distinguish 1 large vs 3 small gaps
```

**Affine Penalty (BETTER):**
```
Cost = open_penalty + extend_penalty × (length-1)

Example gap of 3:
= -12 (opening) + (-2) × 2 (extending) = -16

Biology: One mutation event = one gap (preferred)
         Three events = three gaps (penalized more)
```

**Typical Values:**
```
DNA:       Match +5, Mismatch -4, Gap open -12, extend -1
Protein:   Match +1 (via matrix), Gap open -12, extend -2
```

---

## Scoring Matrices for Proteins

### PAM Matrices

| Matrix | Meaning | Use For | Divergence |
|--------|---------|---------|-----------|
| **PAM1** | 1% identity change | N/A (too similar) | Recent (same year) |
| **PAM30** | 30% cumulative change | Recent orthologs | ~30 million years |
| **PAM70** | 70% cumulative change | Moderate divergence | ~70 million years |
| **PAM250** | 250% cumulative change | Distant homologs | Ancient divergence |

**Rule**: Higher PAM = more ancient divergence expected

### BLOSUM Matrices (More Common Now)

| Matrix | Meaning | Use For | Divergence |
|--------|---------|---------|-----------|
| **BLOSUM90** | 90% similarity blocks | Very recent | Days-months |
| **BLOSUM62** | 62% similarity blocks | General purpose | Recent-moderate |
| **BLOSUM45** | 45% similarity blocks | Distant | Ancient |

**Rule**: Higher BLOSUM = more similar sequences expected
**Most Common**: BLOSUM62 (good for most searches)

### Quick Selection Guide

```
Question: How divergent are sequences expected to be?
├─ Recent divergence (same species orthologs)
│  └─ Use: BLOSUM90 or PAM30
├─ Moderate divergence (different species)
│  └─ Use: BLOSUM62 or PAM70 (DEFAULT)
├─ Ancient divergence (distant homologs)
│  └─ Use: BLOSUM45 or PAM250
└─ DNA only
   └─ Use: Simple match/mismatch (5/-4)
```

### How Scoring Matrices Work

```
BLOSUM62 example values:
A-A: +4  (good, both alanine)
A-V: +1  (okay, both nonpolar)
A-D: -2  (bad, opposite charge)
A-W: -3  (very bad, very different)

Total alignment score = sum of all position scores
```

---

## MULTIPLE SEQUENCE ALIGNMENT (Comparing 3+ Sequences)

### Overview
- **Input**: 3 or more sequences
- **Output**: Aligned sequences showing positions and conservation
- **Challenge**: Cannot use simple 2D matrix (3D is too expensive)
- **Solution**: Use heuristics (progressive, iterative, consistency)

### Decision Tree: Which Algorithm?

```
Number of sequences?
├─ <50 sequences
│  └─ Speed/accuracy balance needed?
│     ├─ Speed → MUSCLE
│     ├─ Accuracy → T-Coffee
│     └─ Quick → ClustalW
├─ 50-500 sequences
│  └─ Use MUSCLE or MAFFT
└─ >500 sequences
   └─ Use MAFFT (fastest)
```

---

## Progressive Alignment Strategy

### Concept
Build MSA step-by-step from pairwise comparisons

### Steps

**1. Calculate Pairwise Distances**
```
Seq A vs B: 90% similar
Seq A vs C: 70% similar
Seq B vs C: 65% similar

Most similar: A-B (start with these)
Least similar: B-C (add this last)
```

**2. Build Guide Tree**
```
        ┌─ C (68% to A)
    ┌───┤
────┤   └─ B (90% to A)
    │
    └──── A
    
Start with closest, progressively add distant
```

**3. Align Pairwise (Most Similar First)**
```
Align A + B first (high confidence)
```

**4. Add Third Sequence to Profile**
```
Create profile from A-B alignment
Align C to this profile
```

**5. Continue Adding Sequences**
```
Add D to (A-B-C) alignment
Add E to (A-B-C-D) alignment
...
```

### Advantage vs Disadvantage

**Advantages:**
- Fast (O(n²) not exponential)
- Works for hundreds of sequences
- Converges quickly

**Disadvantage:**
- Early errors propagate
- Not optimal (heuristic)
- Can be trapped in local optima

---

## Common MSA Algorithms Summary

| Algorithm | Speed | Accuracy | Best For | Known For |
|-----------|-------|----------|----------|-----------|
| **ClustalW** | Fast | Good | 10-100 seq | Pioneer, widely used |
| **MUSCLE** | Very fast | Very good | 50-500 seq | Balance of speed/accuracy |
| **MAFFT** | Fastest | Excellent | 100-1000+ | Very large datasets |
| **T-Coffee** | Slow | Best | <50 seq | Consistency-based |

**Default choice**: MUSCLE (good everywhere)
**If speed critical**: MAFFT
**If accuracy critical**: T-Coffee

---

## Expected Value (E-value)

### What is E-value?

**Definition**: Probability that alignment score occurred by pure chance

### Interpretation Guide

| E-value | Meaning | Homology? | Use It? |
|---------|---------|-----------|---------|
| **< 0.0001** | 1 in 10,000 chance | YES, certain | ✓ Use |
| **0.0001-0.01** | 1 in 100 chance | YES, likely | ✓ Use |
| **0.01-0.1** | 1 in 10 chance | MAYBE | ? Investigate |
| **0.1-1** | Marginal | Probably not | ✗ Skip |
| **> 1** | Common by chance | NO | ✗ Skip |

**Rule of Thumb**: E-value < 0.001 = Probably homologous

### E-value vs Percent Identity

**Don't confuse!**
```
Score: A vs B = 80% identity, E = 0.5 (NOT significant)
        C vs D = 50% identity, E = 1e-50 (HIGHLY significant)

Why different? Length! Longer sequences = smaller E-values
```

**Lesson**: ALWAYS check E-value, not just % identity

---

## Identity Percentage Guidelines

| Identity | Conclusion | Reliability |
|----------|-----------|-------------|
| **>95%** | Same gene (allelic) | Very high |
| **85-95%** | Same gene (strain difference) | Very high |
| **70-85%** | Different species, same gene | High |
| **50-70%** | Likely homologous | Medium |
| **<50%** | Uncertain (check E-value) | Low |

**BUT**: Always verify with E-value, especially for short sequences!

---

## Conservation Patterns in MSA

### Reading Alignments

```
Alignment:
Seq1: MVLHYTNQKPLKPPNRT
Seq2: MVLHYTNQKPLKPPNRT
Seq3: MVLHYTAQKPLKPPNRT
Seq4: MVLHYTNQKPLKPPNRT
      ||||||| ||||||||||
      ╔══════════════════╗
      │ Highly conserved │
      │ (functionally    │
      │  important)      │
      ╚══════════════════╝
```

**Interpretation:**
- All identical = critical for function
- Some variation OK = not essential
- High variation = flexible

### Variable vs Conserved Regions

```
Conserved regions:
└─ Active sites
└─ Structural core
└─ Binding sites
└─ Functionally critical

Variable regions:
└─ Surface loops
└─ Linker regions
└─ Non-functional parts
└─ Evolutionary flexible
```

---

## Common Alignment Mistakes & Fixes

| Mistake | Why Wrong | Fix |
|---------|-----------|-----|
| Using global on very different lengths | Will create meaningless gaps | Use local (SW) instead |
| Trusting % identity alone | High % on short seq = chance | Check E-value always |
| Not checking E-values | Can find false homologs | Must verify significance |
| Comparing scores across different pairs | Not normalized | Use E-values for comparison |
| Assuming all gaps = real indels | Can be alignment artifacts | Visually inspect, check other tools |
| Trusting default parameters | May not fit your data | Consider changing if data unusual |
| One alignment = truth | Can be multiple local optima | Try different parameters |
| Ignoring sequence quality | Garbage in = garbage out | Check sequences first |

---

## Memory Aids & Mnemonics

### ALIGNMENT TYPES: **GLoLo**
- **G** = Global (whole sequences)
- **L** = Local (parts only)
- Sometimes written: **GL² = G(lobal) + L(ocal)²**

### NEEDLEMAN-WUNSCH: **3M**
- **M** = Matrix (create DP matrix)
- **M** = Middle (fill all cells)
- **M** = Max (traceback for best path)

### SMITH-WATERMAN: **F3T**
- **F** = Fill matrix (like NW)
- **T** = Track highest score (find it!)
- **T** = Traceback from max (not corner!)

### SCORING MATRICES: **PB** (PAM or BLOSUM)
- **P** = PAM (older, point accepted mutations)
- **B** = BLOSUM (newer, blocks of sequences)
- **Default**: BLOSUM62

### GAP PENALTY: **AOE**
- **A** = Affine is better than linear
- **O** = Open penalty (initial gap cost)
- **E** = Extend penalty (continuing gap cost)
- **Affine formula**: Open + (Extend × length)

### E-VALUE: **SMALL = GOOD**
- Smaller E-value = more significant
- E < 0.001 = Good
- E > 0.1 = Bad

### MSA ALGORITHMS: **CMT**
- **C** = ClustalW (first, standard)
- **M** = MUSCLE (modern, balanced)
- **T** = T-Coffee (best accuracy, slow)
- Also: MAFFT (fastest for huge datasets)

---

## Quick Decision Tables

### Choose Alignment Algorithm

**Given**: Two sequences, different lengths (500 vs 2000bp)

```
Step 1: How many sequences? → 2 (pairwise)
Step 2: Similarity? → Partial homology expected
Step 3: Focus? → Find conserved domain
Decision: Use LOCAL (Smith-Waterman)
Reason: Different lengths + partial match + domain focus
```

**Given**: 50 protein sequences, 350 aa each

```
Step 1: How many sequences? → 50
Step 2: Time constraint? → None
Step 3: Accuracy needed? → Good
Decision: Use MUSCLE
Reason: <100 sequences, fast, accurate enough
Alternative: T-Coffee if best accuracy needed
```

---

## Assessment Questions

### Level 1 (Basic Understanding)
1. What does alignment do?
2. Difference between gap and mismatch?
3. When would you use global vs local?
4. What does E-value measure?

### Level 2 (Intermediate)
1. Explain Needleman-Wunsch in 3 steps
2. Why does Smith-Waterman have a "0" in recurrence?
3. What's the difference between PAM30 and PAM250?
4. Why use affine vs linear gap penalties?

### Level 3 (Advanced)
1. Why does progressive MSA fail sometimes?
2. How do you evaluate MSA quality?
3. What's the relationship between scoring matrix and evolutionary distance?
4. How would you design primers using MSA?

---

## Common Operations Checklist

### Before Running Alignment
- [ ] Sequences in FASTA format
- [ ] No ambiguous characters (N, X only where needed)
- [ ] Checked for contamination
- [ ] Removed vector sequences
- [ ] Noted organism and source

### During Alignment
- [ ] Chose correct algorithm (global/local/MSA)
- [ ] Selected appropriate scoring matrix
- [ ] Set gap penalties reasonably
- [ ] Checked output format options

### After Alignment
- [ ] Visually inspected alignment
- [ ] Checked E-values (if BLAST)
- [ ] Looked for conserved regions
- [ ] Verified against known structure (if available)
- [ ] Compared with literature

---

## Key Formulas Quick Reference

### Percent Identity
```
% Identity = (Matches / Alignment_Length) × 100

Example: 47 matches in 50 positions
= (47/50) × 100 = 94% identity
```

### Percent Similarity (Proteins)
```
% Similarity = (Matches + Conservative_Substitutions) / Length × 100

Example: 40 matches + 5 conservative in 50 positions
= (45/50) × 100 = 90% similarity
```

### Affine Gap Penalty
```
Cost = Gap_Open_Penalty + (Gap_Extend_Penalty × (Gap_Length - 1))

Example: 3bp gap, open=-12, extend=-2
= -12 + (-2 × 2) = -16
```

### Alignment Score (Simple)
```
Score = (Matches × Match_Score) + (Mismatches × Mismatch_Score) + Gap_Cost

Example: 45 matches (+1), 5 mismatches (0), one 3bp gap (-12,-2,-2)
= (45×1) + (5×0) + (-12-2-2) = 45 - 16 = 29
```

---

## Tools Quick Reference

| Tool | URL/Access | Best For | Input | Output |
|------|-----------|----------|-------|--------|
| **NCBI BLAST** | https://blast.ncbi.nlm.nih.gov/ | Quick pairwise | FASTA | Alignments + E-values |
| **ClustalW** | ebi.ac.uk/Tools/msa/ | MSA | FASTA | Aligned seq + tree |
| **MUSCLE** | ebi.ac.uk/Tools/msa/ | Fast MSA | FASTA | Aligned sequences |
| **MAFFT** | mafft.cbrc.jp/alignment/ | Large MSA | FASTA | Aligned sequences |
| **Jalview** | jalview.org | Visualization | Aligned seq | Colored, annotated |
| **MEGA** | megasoftware.net | Phylo + alignment | FASTA | Alignment + tree |

---

## Troubleshooting Common Issues

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| Very low alignment score | Sequences not homologous | Check with BLAST first |
| Too many gaps | Wrong algorithm choice | Try other algorithm |
| E-value too high | Sequences too short | Need longer for significance |
| Different tools differ | Algorithm differences | Normal, try 2-3 tools |
| Gaps in wrong places | Poor gap penalties | Adjust open/extend |
| Can't align DNA well | Using protein algorithm | Use DNA-specific tool |
| MSA looks messy | Too divergent | Try different algorithm |

---

## Study Tips

### To Master Alignment:
1. **Understand WHY** (not just HOW)
   - Why do we need it? → Reveals homology
   - Why multiple algorithms? → Different scenarios

2. **Work through examples**
   - Do at least 3 matrices by hand
   - Understand each step

3. **Try different tools**
   - BLAST online (pairwise)
   - ClustalW web (MSA)
   - Jalview (visualization)

4. **Practice interpretation**
   - What does 95% identity mean?
   - What does E-value < 1e-50 mean?
   - Which regions are conserved?

5. **Connect to biology**
   - How would this identify gene function?
   - How would this find disease mutations?
   - How would this build phylogeny?

---

## Exam Preparation

### Likely Questions

**Short Answer:**
- "Explain the difference between global and local alignment"
- "When would you use Smith-Waterman?"
- "Why are gap penalties important?"
- "Interpret this E-value: 3.2e-15"

**Problem Solving:**
- "Given two genes of different length but with 75% identity over conserved region, which alignment would you use?"
- "Design MSA strategy for 100 bacterial sequences"

**Interpretation:**
- "Given alignment output, identify conserved vs variable regions"
- "Predict protein function from MSA of homologs"

### Key Points to Memorize

```
✓ Needleman-Wunsch = Global (whole sequences)
✓ Smith-Waterman = Local (parts only)
✓ BLOSUM62 = Default protein matrix
✓ E-value < 0.001 = Probably homologous
✓ Conservation = Functional importance
✓ Gap penalty = Minimize gaps biologically
```

---

## Final Checklist for Understanding

- [ ] Can explain what sequence alignment is
- [ ] Understand why we need alignment
- [ ] Know when to use global vs local
- [ ] Can explain Needleman-Wunsch algorithm
- [ ] Can explain Smith-Waterman algorithm
- [ ] Understand scoring matrices
- [ ] Know MSA approaches (progressive, iterative)
- [ ] Can choose appropriate algorithm
- [ ] Understand E-values
- [ ] Can interpret alignment output
- [ ] Know tools (BLAST, ClustalW, MUSCLE)
- [ ] Can identify errors in alignments

---

**Last Updated**: 2026
**Scope**: Pairwise and Multiple Sequence Alignment
**Level**: Beginner to Advanced