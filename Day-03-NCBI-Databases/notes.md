# Day 3: NCBI Databases - Quick Recall Notes

## Quick Reference Overview

| Database | Main Content | Unique ID | Format | Primary Use |
|----------|-------------|-----------|--------|------------|
| **GenBank** | DNA/RNA sequences | Accession (NC_, NM_, NP_) | FASTA, GenBank format | Genomic research, sequence analysis |
| **PubMed** | Research literature | PMID (numeric) | Citations + abstracts | Literature review, research discovery |
| **Protein DB** | Protein sequences | NP_/sp| codes | FASTA, UniProt | Protein analysis, function prediction |

---

## NCBI = National Center for Biotechnology Information
- **Established**: 1988
- **Part of**: National Library of Medicine (NLM)
- **Cost**: Free, publicly accessible
- **Coverage**: All organisms, global reach

---

## GenBank - DNA/RNA Repository

### What It Contains
- DNA sequences (genomic, mRNA, EST, tRNA, rRNA)
- Viral, bacterial, plant, animal, fungal sequences
- Whole genomes to single genes
- ~220 million sequences (continuously growing)

### Key IDs & Nomenclature
```
NC_  = Chromosome (complete genome)
NM_  = mRNA sequence
NP_  = Protein product
NT_  = Contig sequence
NZ_  = Whole genome shotgun
Format: NC_000001.11 (Letter-Letter_Numbers.Version)
```

### Essential Record Components
- **Locus**: Unique entry identifier
- **Accession Number**: Permanent ID for citation
- **Definition**: Brief sequence description
- **Source**: Organism info (species, strain)
- **Features**: CDS, genes, promoters, exons/introns
- **ORIGIN**: Actual sequence (ATGC nucleotides)

### Key Features to Know
| Feature | Definition | Importance |
|---------|-----------|-----------|
| **CDS** | Coding sequence (protein-coding) | Marks exons that encode protein |
| **mRNA** | Messenger RNA | Gene expression level data |
| **Promoter** | Regulatory binding site | Controls gene expression |
| **Exon** | Expressed sequence | Present in mature mRNA |
| **Intron** | Intervening sequence | Removed during splicing |

### GenBank Access
- **Web**: Entrez (search interface)
- **Search**: Gene name, organism, keywords
- **Output**: FASTA, GenBank, or graphical format
- **BLAST**: Similarity search tool
- **FTP**: Bulk downloads for bioinformatics

### Quick Search Tips
```
Examples:
"human insulin" → General search
"homo sapiens"[organism] → Filter by species
mRNA[filter] → Filter by sequence type
"cystic fibrosis" AND gene → Specific search
```

### INSDC Partnership
- **GenBank** (USA) = NCBI
- **EMBL** (Europe) = European Molecular Biology Laboratory
- **DDBJ** (Japan) = DNA Data Bank of Japan
→ All three share sequences in real-time

---

## PubMed - Biomedical Literature

### Database Statistics
- **Coverage**: 1946 to present (78+ years)
- **Articles**: 33+ million citations
- **Journals**: 5,600+ biomedical journals
- **Languages**: Multiple (mostly English)
- **Annual additions**: ~1-2 million new articles

### What PubMed Indexes
- Original research articles
- Systematic reviews & meta-analyses
- Case reports
- Review articles
- Clinical trials
- Book chapters
- Conference proceedings

### Record Components
- **PMID**: PubMed ID (numeric, permanent)
- **Title**: Article heading
- **Authors**: Full author list + affiliations
- **Abstract**: Summary of research
- **Keywords**: Author-supplied or MeSH terms
- **DOI**: Digital Object Identifier (linking)
- **Journal**: Publication name (volume, issue, pages)
- **Date**: Publication date (year, month, day)

### MeSH Indexing (Medical Subject Headings)
- **Vocabulary**: ~28,000 standardized terms
- **Hierarchy**: Tree structure, top-down categories
- **Purpose**: Enables precise searching beyond keywords
- **Updated**: Annually by NLM
- **Example Path**: Diseases → Neoplasms → Breast Neoplasms

### PubMed Search Operators - Quick Reference

**Boolean Operators:**
```
AND   = Both terms required
OR    = Either term acceptable
NOT   = Exclude term
""    = Exact phrase (use quotes)
```

**Field Tags (Most Common):**
```
[AU]   = Author (format: "Smith J"[AU])
[TI]   = Title (specific article title)
[AB]   = Abstract (full text search)
[TA]   = Journal name
[MESH] = Medical Subject Heading
[PDAT] = Publication date (YYYY/MM/DD)
```

**Quick Search Examples:**
```
"breast cancer"[TI] AND 2023[PDAT] → 2023 breast cancer titles
Smith J[AU] AND diabetes → Author Smith + diabetes topic
immunotherapy[MESH] → MeSH controlled term
"heart failure"[AB] AND treatment → Phrase in abstract
```

### Filters Available
- **Date Range**: "Last 1 year," "Last 5 years," custom
- **Article Type**: Review, Clinical Trial, Case Report
- **Species**: Human, Mouse, Rat, etc.
- **Language**: English, Spanish, Chinese, etc.
- **Journal**: Specific journal filtering

### Publication Type Quality Ranking
```
Highest: Systematic Reviews, Meta-analyses
High: Randomized Controlled Trials
Medium: Original research, Reviews
Lower: Case reports, Letters, Editorials
```

### Citation Metrics
- **View Count**: How many times viewed
- **Citation Count**: Cited by other papers
- **Altmetric Score**: Social media attention
- **Journal Impact Factor**: Average citations per article
- **h-Index**: Researcher productivity measure

### My NCBI Features
- **Save Searches**: For reuse and tracking
- **Alerts**: Email notifications for new articles
- **Collections**: Organize saved articles
- **Export**: Download in multiple formats (BibTeX, CSV, XML)

### PubMed Central (PMC)
- Free, full-text article archive
- Selected journals available in full
- Greater access than abstract-only PubMed

---

## Protein Database - Protein Sequences

### Database Components
- **Swiss-Prot**: Manually curated, high-quality
- **TrEMBL**: Automatic translations from GenBank
- **RefSeq Proteins**: NCBI-curated reference sequences
- **PIR**: Protein Information Resource entries
- **Patent Sequences**: From patent applications

### Protein ID Formats
```
NP_xxxxxx.x = RefSeq protein (recommended)
sp|P12345|  = Swiss-Prot/UniProt code
gi|123456789| = GenInfo ID (older format)
Format explained:
- First letters indicate source/type
- Numbers are unique identifiers
- Version number tracks updates
```

### Essential Protein Record Components
- **Header**: Protein name, source organism, ID
- **Sequence**: Amino acids (20-letter code) or single-letter code
- **Length**: Number of amino acids
- **Molecular Weight**: Calculated mass in Daltons
- **Composition**: % of each amino acid type

### Key Protein Annotations
| Annotation | Meaning | Usefulness |
|-----------|---------|-----------|
| **GO Terms** | Gene Ontology (function, location, process) | Functional classification |
| **Domains** | Conserved functional regions (kinase, zinc-finger) | Indicates protein family |
| **Orthologs** | Same protein in different species | Evolutionary relationships |
| **Paralogs** | Related proteins in same organism | Functional redundancy |
| **InterPro** | Protein family and domain classification | Structural prediction |

### RefSeq Proteins (Preferred for Research)
- **Accession**: NP_xxxxxx.x format
- **Quality**: Professionally curated, non-redundant
- **Content**: One best sequence per gene
- **Updates**: Synchronized with genome updates
- **Annotation**: Comprehensive functional information

### FASTA Format (Standard Protein Sequence)
```
>gi|123456789|sp|P12345|INSULIN_HUMAN
MALWMRLLPLLALLALWGPDPAAAFVNQHLCGSHLVEALYLVCGERGFFYTPKT
TISLWKRQTLGQHDFSAGEGLYTHMKALRPDEDRLSPLHSVYVDQWDWERVMG
```

### Key Protein Accession Sources
| Source | Reliability | Redundancy | Use Case |
|--------|------------|-----------|----------|
| **RefSeq** | Very High | Non-redundant | Primary choice |
| **Swiss-Prot** | Very High | Non-redundant | Alternative to RefSeq |
| **GenBank CDS Translation** | Medium | Highly redundant | Complete coverage but messy |

### Protein Access Methods
- **Direct Search**: By name, function, or organism
- **BLAST Protein Search** (blastp): Compare against all proteins
- **PSI-BLAST**: Iterative similarity search
- **Sequence Alignment**: Compare multiple proteins

### Related NCBI Databases for Proteins
- **PDB**: 3D structure data
- **MMDB**: Protein structure summaries
- **CDD**: Conserved Domain Database
- These link to protein sequences for integrated analysis

### Amino Acid Code (Quick Reference)
```
3-letter  1-letter  Amino Acid
Ala       A         Alanine
Asp       D         Aspartic acid
Glu       E         Glutamic acid
Phe       F         Phenylalanine
Gly       G         Glycine
His       H         Histidine
Ile       I         Isoleucine
Lys       K         Lysine
Met       M         Methionine
Asn       N         Asparagine
Pro       P         Proline
Gln       Q         Glutamine
Arg       R         Arginine
Ser       S         Serine
Thr       T         Threonine
Val       V         Valine
Trp       W         Tryptophan
Tyr       Y         Tyrosine
Cys       C         Cysteine
```

---

## Database Interconnections

### The Integration Web
```
GenBank (DNA/RNA) ↔ Protein DB ← Translation relationship
    ↓                    ↓
    └──→ PubMed (Literature) ←
         All cross-referenced
         via Taxonomy
```

### Cross-linking Examples
- **GenBank → Protein**: CDS features automatically translated
- **GenBank/Protein → PubMed**: Links to describing publications
- **All → Taxonomy**: Organized by organism classification
- **All → Literature**: References to research basis

### Research Workflow Integration
```
Step 1: Find gene in GenBank
Step 2: Retrieve protein translation automatically
Step 3: Search PubMed for function studies
Step 4: Compare proteins across species
Step 5: Validate findings in literature
```

---

## Search Strategies by Research Goal

### Goal: Find a Specific Gene
**Strategy**: GenBank search
```
Search: "BRCA1"[gene] AND "homo sapiens"[organism]
Check: Accession number, sequence features
Result: Precise genomic coordinates and structure
```

### Goal: Review Literature on Topic
**Strategy**: PubMed search
```
Search: "immunotherapy"[MESH] AND "response"[AB] AND 2022[PDAT]
Filter: Review Articles, Human studies
Result: Comprehensive literature overview
```

### Goal: Compare Proteins Across Species
**Strategy**: BLAST protein search
```
1. Get protein sequence from NCBI Protein DB
2. Run BLAST against Protein DB
3. Compare hits across species
Result: Orthologs and evolutionary relationships
```

### Goal: Predict Protein Function
**Strategy**: Integrated approach
```
1. Get protein sequence (Protein DB)
2. Search BLAST for similar proteins
3. Find literature on homologs (PubMed)
4. Check domains and annotations
Result: Evidence-based function prediction
```

### Goal: Track Disease Mutations
**Strategy**: GenBank + PubMed combined
```
1. Find normal gene in GenBank
2. Locate mutations in disease studies (PubMed)
3. Compare sequences
4. Link to structure impact
Result: Pathogenic mutation database
```

---

## Citation & Reproducibility Essentials

### What to Record (Always!)
```
For Sequences:
- Accession Number (e.g., NC_000001.11)
- Access Date (e.g., January 15, 2024)
- Database Version (e.g., GenBank Release 250)
- Organism name (full binomial: Homo sapiens)

For Literature:
- PMID (unique identifier)
- Full citation (authors, year, journal)
- Access date
- URL or DOI

For Proteins:
- Accession number
- Protein ID version
- Source database (RefSeq, Swiss-Prot, etc.)
```

### Example Citation Format
```
The human BRCA1 gene sequence (Accession: NC_000017.11) was 
retrieved from GenBank on January 15, 2024. The corresponding 
protein sequence (NP_009060.2) was obtained from NCBI Protein 
Database. Functional studies were identified through PubMed 
(PMID: 12345678).
```

---

## Common Mistakes to Avoid

| Mistake | Problem | Solution |
|---------|---------|----------|
| Using protein in gene search | Wrong sequence type retrieved | Use organism + type filter |
| Not noting Accession # | No reproducibility | Always record version numbers |
| Assuming all annotations correct | Outdated/incorrect info | Cross-check with literature |
| Ignoring sequence redundancy | Multiple copies confuse results | Use RefSeq/Swiss-Prot |
| Using old sequence versions | Outdated annotations | Check version numbers |
| Searching PubMed without date filter | Overwhelming results | Use date range and MeSH |
| Confusing mRNA with genomic DNA | Include introns/exons mix-up | Check feature annotations |

---

## Memory Aids & Mnemonics

### NCBI Database Trinity: **GePP**
- **G** = GenBank (Genomic sequences)
- **P** = PubMed (Publications/literature)
- **P** = Protein Database (Proteins)

### GenBank Record: **LADS**
- **L** = Locus (unique identifier)
- **A** = Accession (permanent ID)
- **D** = Definition/Description
- **S** = Source (organism info)

### PubMed Essentials: **MAD**
- **M** = MeSH (controlled vocabulary)
- **A** = Authors (who wrote it)
- **D** = Date (when published)

### Protein Quality: **CRP**
- **C** = Curated (manually reviewed)
- **R** = RefSeq (NCBI standard)
- **P** = Preferred (use these first)

### Database Access: **BFE**
- **B** = BLAST (similarity search)
- **F** = FTP (bulk downloads)
- **E** = Entrez (web search interface)

---

## Quick Assessment Questions

### GenBank Level
1. What distinguishes NM_ from NC_ accession types?
2. How are CDS features different from exons?
3. What is the INSDC and who participates?

### PubMed Level
1. Name 3 MeSH-searchable fields in PubMed
2. What advantages do systematic reviews have over case reports?
3. How to filter for recent articles in specific journals?

### Protein DB Level
1. Why is RefSeq preferred over GenBank translations?
2. What information do GO terms provide?
3. How do orthologs differ from paralogs?

### Integration Level
1. Trace path: Gene discovery → Literature → Protein analysis
2. How does taxonomy connect all three databases?
3. Why should protein version match genome version?

---

## Quick Links (Know Where to Go)

| Resource | URL Ending | Purpose |
|----------|-----------|---------|
| GenBank | /genbank/ | Sequence database |
| PubMed | /pubmed/ | Literature search |
| Protein | /protein/ | Protein sequences |
| BLAST | /blast/ | Similarity searching |
| Entrez | /search/ | Unified search interface |
| Taxonomy | /taxonomy/ | Organism classification |
| My NCBI | /myncbi/ | User accounts & saved searches |

---

## Key Formulas & Calculations

**Molecular Weight Estimation (Proteins):**
- Average amino acid MW ≈ 120 Da
- Protein MW ≈ (# amino acids × 120) - 18 Da (water loss)
- Example: 100 aa protein ≈ 11.8 kDa

**GC Content (DNA/RNA):**
- GC% = [(G count + C count) / Total bases] × 100
- Higher GC% = more stable, higher melting point

**Codon Translation Rule:**
- 1 codon (3 nucleotides) = 1 amino acid
- Protein length ≈ Gene length ÷ 3 (minus stop codons)

---

## Final Checklist for Database Use

**Before Starting Research:**
- [ ] Define search question clearly
- [ ] Choose appropriate database(s)
- [ ] Identify correct organism(s)
- [ ] Note publication date range needed
- [ ] Plan search strategy

**During Database Search:**
- [ ] Record all Accession numbers
- [ ] Note version numbers
- [ ] Save all relevant citations
- [ ] Screenshot key results
- [ ] Cross-check multiple databases

**After Finding Data:**
- [ ] Verify sequence accuracy
- [ ] Check annotation dates
- [ ] Cross-reference with literature
- [ ] Document search strategy
- [ ] Prepare proper citations

---

**Last Updated**: 2026
**Scope**: NCBI databases (GenBank, PubMed, Protein DB)
**Level**: Intermediate undergraduate to graduate students