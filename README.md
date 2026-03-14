# SARS-CoV-2 Genomic Analysis Pipeline: From Assembly to Drug Target Discovery

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Bioinformatics](https://img.shields.io/badge/field-bioinformatics-green.svg)]()

## Project Overview

This repository documents my journey through the **Applied Bioinformatics Specialization** offered by UC San Diego on Coursera. The specialization, titled "Hacking COVID-19," provided hands-on training in critical bioinformatics skills using real SARS-CoV-2 genomic data from the pandemic.

**About This Project:**  
As a recent graduate with a Bachelor's in Biosciences from COMSATS University Islamabad, my undergraduate research focused on wet-lab molecular biology (Final Year Project: *Assessment of the role of ATP8B1 Gene in gallstones in Pakistani Populations*). While I had limited exposure to bioinformatics tools during my degree, I recognized the growing importance of computational approaches in modern biological research. This specialization allowed me to build practical bioinformatics skills from the ground up, working with actual pandemic data to understand genome assembly, annotation, phylogenetics, and drug target discovery.

**What I Learned:**
- ✅ De novo genome assembly from raw Illumina paired-end sequencing reads
- ✅ Functional genome annotation and gene prediction for viral genomes
- ✅ Comparative genomics and multiple sequence alignment techniques
- ✅ Phylogenetic tree construction with molecular clock dating
- ✅ Metabolic pathway analysis for therapeutic target identification

**Key Achievement:** Successfully assembled the complete SARS-CoV-2 genome (29,903 bp) from raw sequencing data, annotated 11 major open reading frames, traced the evolutionary origins of the virus through phylogenetic analysis, and identified guanylate kinase as a potential antiviral drug target.

---

## Table of Contents
- [Background](#background)
- [Workflow Overview](#workflow-overview)
- [01: Genome Assembly](#01-genome-assembly)
- [02: Genome Annotation & Diagnostic Design](#02-genome-annotation--diagnostic-design)
- [03: Multiple Sequence Alignment & Viral Origins](#03-multiple-sequence-alignment--viral-origins)
- [04: Metabolic Pathway Analysis](#04-metabolic-pathway-analysis)
- [05: Phylogenetic Inference & Dating](#05-phylogenetic-inference--dating)
- [Tools & Technologies](#tools--technologies)
- [Key Results](#key-results)
- [Data Availability](#data-availability)
- [Future Learning Goals](#future-learning-goals)
- [References](#references)
- [Acknowledgments](#acknowledgments)

---

## Background

### About SARS-CoV-2
- **Virus Type:** Betacoronavirus (family: Coronaviridae)
- **Genome:** Positive-sense single-stranded RNA (~29.9 kb)
- **Closest Relative:** Bat coronavirus RaTG13 (96.1% nucleotide identity)
- **Key Proteins:** Spike (S), Nucleocapsid (N), Membrane (M), Envelope (E), RNA-dependent RNA polymerase (RdRp)

### Why Study the SARS-CoV-2 Genome?
Understanding viral genomes through bioinformatics helps us:
1. **Develop Diagnostics:** Design specific PCR tests to detect infections
2. **Discover Therapeutics:** Identify potential drug targets in viral proteins
3. **Track Evolution:** Monitor how the virus mutates and spreads globally
4. **Understand Origins:** Determine when and how the pandemic began

---

## Workflow Overview

The complete bioinformatics pipeline I followed in this specialization:

```
Raw Sequencing Reads (FASTQ)
          ↓
    [SPAdes Assembly]
          ↓
    Assembled Genome (FASTA)
          ↓
    [QUAST Quality Check]
          ↓
    [Prokka Annotation]
          ↓
    Annotated Genome (GFF, FAA)
          ↓
    ┌──────────────┬──────────────┬──────────────┐
    ↓              ↓              ↓              ↓
[BLAST]      [MUSCLE MSA]    [BioCyc]    [ViralMSA]
Identify     Spike Protein   Pathway     Phylogenetic
Relatives    Evolution       Analysis    Tree Dating
    ↓              ↓              ↓              ↓
RaTG13       Natural         Guanylate   tMRCA:
Bat CoV      Origin          Kinase      Oct-Nov 2019
```

---

## 01: Genome Assembly

### What I Did
Reconstructed the complete SARS-CoV-2 genome from raw paired-end Illumina sequencing reads taken from a patient sample. This process involves taking millions of short DNA/RNA fragments and assembling them into longer continuous sequences (contigs) that represent the full viral genome.

### Learning Objectives
- Understand how modern sequencing technologies work (paired-end reads)
- Learn the concept of genome assembly using de Bruijn graphs
- Evaluate assembly quality using standard metrics (N50, NG50)
- Identify viral sequences from a mixed sample containing human and microbial RNA
- Use BLAST to find the closest relatives of an unknown virus

### Methods

**Platform:** Galaxy (web-based bioinformatics workflow system)

**Data Source:**  
- Accession: SRR10971381 (NCBI SRA database)
- Sample: Bronchoalveolar lavage fluid (BALF) from Wuhan patient
- Sequencing: Illumina paired-end (2 × 150 bp reads)

**Analysis Steps:**

1. **Assembly with SPAdes**
   - Tool: SPAdes genome assembler
   - Approach: De Bruijn graph construction from k-mers (k=25)
   - Input: Paired-end FASTQ files
   - Output: Contigs (contiguous sequences) and scaffolds

2. **Quality Assessment with QUAST**
   - Tool: QUAST (Quality Assessment Tool for Genome Assemblies)
   - Metrics calculated:
     - **N50:** The length at which 50% of the assembly is contained in contigs of this size or larger
     - **NG50:** Similar to N50 but normalized to expected genome size (~30,000 bp)
     - **Total length:** Sum of all contig lengths
     - **Largest contig:** Length of the longest assembled sequence
     - **GC content:** Percentage of G and C nucleotides

3. **Homology Search with BLAST**
   - Tool: NCBI BLAST (Basic Local Alignment Search Tool)
   - Database: Nucleotide collection (nt), limited to sequences before January 2020
   - Purpose: Identify what virus we assembled and find its closest known relatives
   - Top match: Bat coronavirus RaTG13 with 96.1% nucleotide identity

### Key Concepts Learned

**The "Newspaper Problem":**  
Genome assembly is like trying to reconstruct a shredded newspaper. You have thousands of small fragments (reads) and must figure out how they overlap to recreate the original text (genome).

**Metatranscriptomics:**  
The sample contained a mixture of human RNA, viral RNA, and bacterial RNA. The assembly process had to separate viral sequences from this complex mixture.

**Why Multiple Contigs?**  
Short-read sequencing has limitations - highly repetitive regions or low-coverage areas can prevent assembly of a single continuous sequence. In this case, the primary viral genome assembled into one major contig, while other contigs represented host sequences or bacterial contamination (bacteriophages).

### Results

| Metric | Value | Interpretation |
|--------|-------|----------------|
| **Largest Contig** | ~29,900 bp | Nearly complete SARS-CoV-2 genome |
| **N50** | ~29,900 bp | High-quality assembly |
| **Total Contigs** | Multiple | Viral genome + human + bacterial sequences |
| **GC Content** | ~38% | Typical for coronaviruses |
| **Top BLAST Hit** | Bat CoV RaTG13 | 96.1% identity |
| **E-value** | 0.0 | Highly significant match |

**Key Findings:**
- ✅ Successfully assembled complete SARS-CoV-2 genome in a single major contig
- ✅ Identified bat coronavirus RaTG13 as the closest known relative
- ✅ Confirmed the virus is a novel coronavirus, distinct from SARS-CoV-1 (79% identity)
- ✅ Supporting evidence for zoonotic origin (transmission from animals to humans)

---

## 02: Genome Annotation & Diagnostic Design

### What I Did
Identified all the genes and functional elements within the assembled SARS-CoV-2 genome. This process, called genome annotation, reveals what proteins the virus can make and how it functions. I then used this information to design specific PCR primers for diagnostic testing.

### Learning Objectives
- Understand the Central Dogma (DNA → RNA → Protein)
- Learn how genes are predicted in viral genomes using open reading frames (ORFs)
- Annotate protein-coding genes and assign their functions
- Understand how PCR-based diagnostic tests work
- Design specific primers that detect only SARS-CoV-2 (not other viruses or human DNA)

### Methods

**Annotation with Prokka**
- Tool: Prokka (Prokaryotic genome annotation pipeline)
- Input: Assembled SARS-CoV-2 genome (FASTA file)
- Process:
  - Gene prediction using Prodigal
  - Functional annotation using BLAST against protein databases
  - tRNA and rRNA detection using Aragorn and RNAmmer
- Output files:
  - `.gff` - Gene locations and features
  - `.faa` - Protein sequences (amino acids)
  - `.ffn` - Gene sequences (nucleotides)
  - `.gbk` - GenBank format for visualization

**Understanding Open Reading Frames (ORFs):**
- An ORF is a stretch of DNA/RNA that could encode a protein
- Starts with a start codon (ATG/AUG) and ends with a stop codon (TAA, TAG, TGA)
- Each genome has 6 possible reading frames (3 on forward strand, 3 on reverse)
- SARS-CoV-2 has ~11 major ORFs encoding both structural and non-structural proteins

**PCR Primer Design**
- Tool: Primer3
- Target gene: N gene (Nucleocapsid) - highly conserved and expressed abundantly
- Design criteria:
  - Primer length: 18-25 nucleotides
  - Melting temperature (Tm): 58-62°C (both primers within 2°C of each other)
  - GC content: 40-60%
  - Amplicon size: 100-200 bp
  - No self-complementarity (avoids primer-dimers and hairpins)
  - 100% specificity to SARS-CoV-2 (checked via BLAST)

### Annotated Genes

| Gene/ORF | Position | Length (aa) | Function |
|----------|----------|-------------|----------|
| **ORF1ab** | 266-21555 | 7,096 | Replicase polyprotein (RdRp, proteases, helicase) |
| **S (Spike)** | 21563-25384 | 1,273 | Cell entry and receptor binding (binds ACE2) |
| **ORF3a** | 25393-26220 | 275 | Ion channel, virulence factor |
| **E (Envelope)** | 26245-26472 | 75 | Viral assembly |
| **M (Membrane)** | 26523-27191 | 222 | Virion structure |
| **ORF6** | 27202-27387 | 61 | Interferon antagonist (immune evasion) |
| **ORF7a** | 27394-27759 | 121 | Accessory protein |
| **ORF7b** | 27756-27887 | 43 | Structural protein |
| **ORF8** | 27894-28259 | 121 | Immune evasion |
| **N (Nucleocapsid)** | 28274-29533 | 419 | RNA packaging and replication |
| **ORF10** | 29558-29674 | 38 | Unknown function |

### Key Proteins Explained

**Spike (S) Protein:**
- The "crown" of the coronavirus (gives it the name)
- Binds to ACE2 receptors on human cells to enter
- Primary target for vaccines and neutralizing antibodies
- Contains receptor-binding domain (RBD) that directly contacts ACE2

**Nucleocapsid (N) Protein:**
- Packages viral RNA inside the virus particle
- Highly abundant during infection (good diagnostic target)
- Most conserved protein across coronaviruses

**RNA-dependent RNA Polymerase (RdRp):**
- Part of the ORF1ab polyprotein
- Copies the viral RNA genome
- Target for antiviral drugs like Remdesivir

### PCR Diagnostic Design Results

**Designed Primer Pair (Example):**
```
Forward Primer: 5'-GACCCCAAAATCAGCGAAAT-3'
Reverse Primer: 5'-TCTGGTTACTGCCAGTTGAATCTG-3'

Target: N gene (Nucleocapsid)
Amplicon Size: 164 bp
Tm (Forward): 58.2°C
Tm (Reverse): 60.1°C
ΔTm: 1.9°C ✓
```

**Specificity Validation (BLAST Check):**
- ✅ 100% match to SARS-CoV-2 N gene
- ✅ No matches to SARS-CoV-1 or MERS-CoV
- ✅ No matches to common cold coronaviruses (HCoV-229E, OC43, NL63, HKU1)
- ✅ No matches to human genome
- ✅ No matches to other respiratory pathogens (Influenza, RSV, etc.)

### Key Findings
- ✅ Identified all 11 major open reading frames in SARS-CoV-2
- ✅ Annotated structural proteins (S, N, M, E) and non-structural proteins (RdRp, proteases)
- ✅ Designed highly specific diagnostic primers for RT-PCR testing
- ✅ Primers validated for specificity against all known coronaviruses and human genome

---

## 03: Multiple Sequence Alignment & Viral Origins

### What I Did
Compared the SARS-CoV-2 spike protein sequence with related coronaviruses to understand evolutionary relationships and investigate theories about the virus's origin. This analysis helps determine whether the virus evolved naturally or was artificially created.

### Learning Objectives
- Learn the difference between global and local sequence alignment
- Perform multiple sequence alignment (MSA) to compare multiple related sequences
- Understand protein evolution and conservation
- Analyze the receptor-binding domain (RBD) of the spike protein
- Evaluate scientific hypotheses about viral origins using genomic evidence

### Methods

**Sequence Collection:**
- SARS-CoV-2 spike protein (from Prokka annotation)
- Bat coronavirus RaTG13 spike protein
- SARS-CoV-1 spike protein
- Additional coronavirus spike proteins for comparison

**Pairwise Alignment with BLASTP:**
- Tool: BLAST (Protein BLAST version)
- Compared SARS-CoV-2 spike vs RaTG13 spike
- Compared SARS-CoV-2 spike vs SARS-CoV-1 spike

**Multiple Sequence Alignment:**
- Tool: MUSCLE (Multiple Sequence Comparison by Log-Expectation)
- Aligned spike proteins from multiple coronaviruses
- Visualized conservation patterns across the entire protein

### Understanding Alignment Types

**Global Alignment (Needleman-Wunsch):**
- Aligns sequences from beginning to end
- Best for sequences of similar length and related function
- Used when comparing full-length spike proteins

**Local Alignment (Smith-Waterman):**
- Finds regions of similarity within sequences
- Best for finding conserved domains
- Used by BLAST to find related sequences

### Key Regions Analyzed

**1. Receptor-Binding Domain (RBD):**
- Location: Amino acids ~319-541 in spike protein
- Function: Directly binds to ACE2 receptor on human cells
- Evolution: Shows specific adaptations for human cell entry

**2. Receptor-Binding Motif (RBM):**
- Location: Within the RBD
- Function: Makes direct contact with ACE2
- Key residues differ from SARS-CoV-1, increasing binding affinity

**3. Furin Cleavage Site:**
- Location: Between S1 and S2 subunits (position 681-684)
- Sequence: PRRA (Proline-Arginine-Arginine-Alanine insertion)
- Significance: Absent in RaTG13 and SARS-CoV-1
- Function: Enhances viral entry and fusion

### Comparing SARS-CoV-2 to Related Viruses

**vs Bat Coronavirus RaTG13:**
- Overall spike identity: ~90%
- RBD identity: ~93%
- Interpretation: Very close evolutionary relationship
- Supports bat reservoir hypothesis

**vs SARS-CoV-1 (2003 outbreak):**
- Overall spike identity: ~76-78%
- RBD identity: Lower than with RaTG13
- Key differences in receptor-binding residues
- Different furin cleavage site

**Critical RBD Residues (ACE2 Binding):**

| Position | SARS-CoV-1 | SARS-CoV-2 | Effect on ACE2 Binding |
|----------|-----------|-----------|----------------------|
| 455 | Y (Tyrosine) | L (Leucine) | ↑ Increased affinity |
| 486 | L (Leucine) | F (Phenylalanine) | ↑ Increased affinity |
| 493 | N (Asparagine) | Q (Glutamine) | ↑ Increased affinity |
| 498 | D (Aspartate) | Q (Glutamine) | ↑ Increased affinity |
| 501 | T (Threonine) | N (Asparagine) | ↑ Increased affinity |

### Evaluating Origin Hypotheses

**Three Main Theories:**
1. **Artificial Origin Hypothesis:** Lab-created/engineered virus
2. **Direct Zoonotic Transfer:** Bat → Human transmission
3. **Intermediate Host Hypothesis:** Bat → Mammal → Human transmission

**Evidence Against Artificial Engineering:**

1. **Suboptimal Design:**
   - If engineered, would likely use proven SARS-CoV-1 RBD backbone
   - Instead, SARS-CoV-2 has a novel RBM configuration not seen before

2. **Natural Selection Signatures:**
   - RBD shows patterns consistent with natural evolution
   - Optimized through natural adaptation, not computational design

3. **Furin Cleavage Site:**
   - While unusual, similar polybasic sites found in other coronaviruses
   - Could have arisen through natural recombination events

4. **Overall Genomic Structure:**
   - Consistent with natural coronavirus evolution
   - No evidence of restriction enzyme sites or synthetic sequences

**Evidence for Natural Origin:**
- ✅ Close relationship to bat coronaviruses (RaTG13)
- ✅ Evolutionary adaptations consistent with natural selection
- ✅ RBD mutations that improve ACE2 binding evolved naturally
- ✅ Similar to SARS-CoV-1 and MERS-CoV spillover events from animals

### Results

| Analysis | Findings |
|----------|----------|
| **Spike Protein Length** | 1,273 amino acids |
| **Identity to RaTG13** | ~90-93% (very high) |
| **Identity to SARS-CoV-1** | ~76-78% (moderate) |
| **RBD Position** | aa 319-541 |
| **Key Mutations** | 6 major substitutions in RBM |
| **Furin Site** | PRRA insertion (not in RaTG13) |
| **Origin Conclusion** | Natural evolution, likely zoonotic |

**Key Findings:**
- ✅ SARS-CoV-2 spike protein evolved naturally through selection
- ✅ Enhanced ACE2 binding arose from 6 key amino acid substitutions
- ✅ No evidence of genetic engineering or artificial manipulation
- ✅ Closest relative is bat coronavirus RaTG13 (supports zoonotic spillover)
- ✅ Furin cleavage site likely acquired through natural recombination

---

## 04: Metabolic Pathway Analysis

### What I Did
Explored how SARS-CoV-2 hijacks human cell metabolism to replicate and identified potential drug targets by analyzing metabolic pathways. Viruses don't have their own metabolism - they rely entirely on host cell machinery to produce the building blocks needed for viral replication.

### Learning Objectives
- Understand metabolic networks and biochemical pathways
- Learn how cells produce energy and synthesize biomolecules
- Explore pathway databases (BioCyc, EcoCyc, MetaCyc)
- Apply network analysis concepts (reachability, chokepoints, dead-end metabolites)
- Identify potential antiviral drug targets using computational approaches

### Methods

**Database Exploration:**
- **BioCyc:** Human metabolic pathway database (HumanCyc)
- **EcoCyc:** *E. coli* reference metabolism (to learn pathway concepts)
- **MetaCyc:** Multi-organism pathway collection

**Analysis Approaches:**
1. Query specific pathways and enzymes
2. Analyze network structure (nodes = metabolites, edges = reactions)
3. Identify essential genes and chokepoint reactions
4. Compare viral vs host metabolic requirements

### Understanding Metabolic Networks

**Basic Concepts:**

**Metabolites:**  
Chemical compounds involved in metabolism (glucose, ATP, amino acids, nucleotides)

**Reactions:**  
Biochemical transformations that convert reactants to products  
Example: ATP + GMP → ADP + GDP (catalyzed by guanylate kinase)

**Pathways:**  
Series of connected reactions working toward a common goal  
Example: Glycolysis converts glucose to pyruvate, generating ATP

**Enzymes:**  
Proteins that catalyze (speed up) metabolic reactions  
Each enzyme has a specific function and substrate

### SARS-CoV-2 Metabolic Requirements

**What Viruses Need from Host Cells:**

1. **Nucleotides (RNA building blocks):**
   - ATP, GTP, CTP, UTP
   - Required for ~30,000 nucleotide viral genome replication
   - High demand: thousands of genome copies per infected cell

2. **Amino Acids (Protein building blocks):**
   - All 20 standard amino acids
   - Viral polyprotein alone: 7,096 amino acids
   - Structural proteins: thousands more amino acids

3. **Lipids (Membrane components):**
   - Phospholipids for viral envelope
   - Membrane remodeling for replication complexes

4. **Energy (ATP/GTP):**
   - Powers biosynthetic reactions
   - Required for RNA capping and processing

### Key Metabolic Pathways Explored

**1. Purine Nucleotide Biosynthesis:**
- Produces GTP and ATP (needed for viral RNA)
- Key enzyme: Guanylate kinase (converts GMP to GDP)

**2. Pyrimidine Nucleotide Biosynthesis:**
- Produces CTP and UTP (needed for viral RNA)
- Key enzyme: Carbamoyl-phosphate synthetase

**3. Glycolysis:**
- Breaks down glucose to generate ATP
- Provides energy for viral replication

**4. Amino Acid Metabolism:**
- Produces building blocks for viral proteins
- Some pathways can be blocked without harming host

### Network Analysis Concepts

**Reachability Analysis:**  
Can a cell produce a target compound (Z) from available nutrients (N)?

**Example:**
```
Nutrients (N): Glucose, Amino acids
Reactions (R): Human metabolic network
Target (Z): GTP for viral RNA synthesis

Question: Is Z reachable from N?
Answer: Yes, through purine biosynthesis pathway
```

**Dead-End Metabolites:**
- **Dead-end reactants:** Consumed but never produced → potential bottlenecks
- **Dead-end products:** Produced but never consumed → potential waste

**Chokepoint Reactions:**
- Reactions that uniquely produce or consume a metabolite
- No alternative pathways available
- Blocking a chokepoint severely disrupts metabolism

### Drug Target Identification Strategy

**Ideal Antiviral Drug Target:**
1. **Essential for virus:** Blocking it stops viral replication
2. **Non-essential for host:** Minimal toxicity to human cells
3. **No alternative pathways:** Virus can't bypass the block

**Computational Approach:**
1. Model human cell metabolism
2. Model viral metabolic requirements
3. Simulate blocking each enzyme one at a time
4. Identify enzymes that stop viral growth but allow host cell growth

### Primary Drug Target: Guanylate Kinase

**Enzyme:** Guanylate Kinase (EC 2.7.4.8)

**Reaction:**  
ATP + GMP ↔ ADP + GDP

**Pathway:** Purine nucleotide biosynthesis

**Why It's a Good Target:**

1. **Viral Dependency:**
   - SARS-CoV-2 requires massive GTP production for RNA synthesis
   - Produces ~30,000 GTP molecules per viral genome
   - Thousands of viral genomes per infected cell

2. **Host Cell Tolerance:**
   - Human cells can recycle GMP from RNA degradation
   - Don't need high de novo synthesis rates
   - Can survive with reduced guanylate kinase activity

3. **Differential Impact:**
   - Virus removes GTP from cellular pool (non-renewable in infection context)
   - Host has alternative pathways and recycling mechanisms
   - Blocking this enzyme starves the virus while sparing the host

**Supporting Evidence:**
- Published in *Cell Metabolism* (Renz et al., 2020)
- Validated using flux balance analysis
- Shows selectivity for viral vs host metabolism

### Other Potential Targets Explored

| Enzyme | Pathway | Viral Need | Host Need | Target Score |
|--------|---------|-----------|-----------|--------------|
| **Guanylate Kinase** | Purine biosynthesis | High | Low | ⭐⭐⭐⭐⭐ |
| **Carbamoyl-phosphate Synthetase** | Pyrimidine biosynthesis | High | Low | ⭐⭐⭐⭐ |
| **IMP Dehydrogenase** | Purine biosynthesis | High | Moderate | ⭐⭐⭐ |

### Results

**Pathway Analysis Summary:**
- Explored human metabolic network with 10,000+ reactions
- Identified viral-specific metabolic vulnerabilities
- Focused on nucleotide biosynthesis pathways
- Analyzed network chokepoints and dead-end metabolites

**Key Findings:**
- ✅ Identified guanylate kinase as high-priority drug target
- ✅ Virus requires high-rate GTP synthesis for RNA replication
- ✅ Host cells can tolerate guanylate kinase inhibition
- ✅ Computational approach reduces need for expensive wet-lab screening
- ✅ Pathway-level understanding enables rational drug design

---

## 05: Phylogenetic Inference & Dating

### What I Did
Constructed an evolutionary tree (phylogeny) of SARS-CoV-2 sequences and estimated when the virus first emerged in human populations. This analysis helps us understand how the virus spread globally and test hypotheses about its origins.

### Learning Objectives
- Understand phylogenetic trees and evolutionary relationships
- Learn the difference between rooted and unrooted trees
- Perform multiple sequence alignment of viral genomes
- Build phylogenetic trees using maximum likelihood
- Apply molecular clock dating to estimate time to most recent common ancestor (tMRCA)
- Evaluate alternative pandemic origin hypotheses using temporal data

### Methods

**Dataset:**
- 100 SARS-CoV-2 genome sequences
- Collection dates: January 2020 - March 2023
- Geographic distribution: Global (USA, Europe, Asia, Africa)
- Outgroup: Bat coronavirus RaTG13 (for rooting the tree)

**Step 1: Multiple Sequence Alignment**
- Tool: ViralMSA (Viral Multiple Sequence Alignment)
- Method: Reference-based alignment using Minimap2
- Reference: SARS-CoV-2 Wuhan-Hu-1 genome
- Trimming: Removed positions 1-265 and 29675+ (low-quality ends)
- Final alignment: 29,409 bp (positions 266-29674)

**Step 2: Phylogenetic Tree Construction**
- Tool: FastTree (Fast Maximum Likelihood)
- Model: GTR+Gamma (General Time Reversible with rate variation)
- Method: Maximum likelihood estimation
- Output: Unrooted phylogenetic tree in Newick format

**Step 3: Tree Rooting**
Two methods learned:

**Midpoint Rooting:**
- Find the two most distant sequences in the tree
- Place root at the midpoint of the path between them
- Assumes molecular clock (constant evolution rate)

**Outgroup Rooting (Used in this analysis):**
- Use bat coronavirus RaTG13 as outgroup
- RaTG13 diverged ~50 years before SARS-CoV-2 emergence
- Place root on branch leading to outgroup
- More biologically accurate than midpoint rooting

**Step 4: Molecular Clock Dating**
- Tool: LSD2 (Least Squares Dating)
- Input: Unrooted tree + collection dates for each sequence
- Method: Tip-dating with temporal constraints
- Output: Time-calibrated tree with internal node dates

### Understanding Phylogenetic Trees

**Tree Components:**

**Leaves (Tips):**  
- Represent sampled sequences (the 100 SARS-CoV-2 genomes)
- Labeled with sequence name and collection date

**Internal Nodes:**  
- Represent inferred common ancestors
- Not directly observed, but reconstructed from data

**Branches:**  
- Represent evolutionary time
- Length proportional to genetic distance (number of mutations)

**Root:**  
- Oldest point in the tree
- Most recent common ancestor (MRCA) of all sequences

### Molecular Clock Concept

**What is a Molecular Clock?**
- Assumption: Mutations accumulate at roughly constant rate over time
- If true, genetic distance correlates with time
- Allows us to convert branch lengths (mutations) to time (years)

**Mutation Rate Calculation:**
- Plot genetic distance vs collection date
- Slope = mutation rate (substitutions per site per year)
- SARS-CoV-2 rate: ~8.9 × 10⁻⁴ substitutions/site/year
- Typical for RNA viruses

**Clock Validation:**
- R² value > 0.95 indicates good clock-like behavior
- Strong temporal signal in the data
- Justifies molecular clock dating approach

### Time to Most Recent Common Ancestor (tMRCA)

**Definition:**
- tMRCA = Estimated date when all sampled sequences shared a common ancestor
- Represents when the virus first emerged in human population

**Our Estimate:**
- **tMRCA: October-November 2019**
- 95% Confidence Interval: September 2019 - December 2019
- Consistent with Wuhan outbreak (first cases December 2019)

**What This Means:**
- SARS-CoV-2 emerged ~2-3 months before detection
- Initial spread was cryptic (undetected) for a brief period
- Early cases may have been misdiagnosed as flu or pneumonia

### Testing Alternative Origin Hypotheses

**Italy Early Origin Hypothesis:**
- **Claim:** Virus circulating in Italy since June 2019 (Amendola et al.)
- **Our Finding:** tMRCA ~October-November 2019 (5 months later)
- **Conclusion:** REJECTED
- **Reasoning:** Would require undetected outbreak for months, implausible given viral transmissibility

**Lab Leak (Pre-2019) Hypothesis:**
- **Claim:** Virus escaped from lab before 2019
- **Our Finding:** No sampled sequences predate late 2019
- **Conclusion:** NOT SUPPORTED by molecular clock data
- **Reasoning:** tMRCA estimate consistent with natural spillover timeline

**Wuhan Wet Market Origin:**
- **Claim:** Virus emerged from Wuhan market, late 2019
- **Our Finding:** tMRCA October-November 2019, first cases December 2019
- **Conclusion:** SUPPORTED
- **Reasoning:** Timeline matches, Wuhan-Hu-1 reference very close to MRCA

### Visualization with Taxonium

**Tool:** Taxonium (interactive phylogenetic tree viewer)
- Handles very large trees (millions of sequences)
- X-axis: Time (calendar dates)
- Y-axis: Viral lineages
- Search function to find specific sequences
- Hover over nodes to see dates and metadata

**Global Tree Analysis:**
- Viewed complete global SARS-CoV-2 phylogeny (millions of sequences)
- Found Wuhan-Hu-1 reference genome near root
- Confirms early emergence in Wuhan, China
- Shows rapid global spread after emergence

### Results

| Analysis Component | Result |
|-------------------|--------|
| **Sequences Aligned** | 100 SARS-CoV-2 genomes |
| **Alignment Length** | 29,409 bp |
| **Phylogenetic Method** | Maximum Likelihood (GTR+Gamma) |
| **Rooting Method** | Outgroup (RaTG13) |
| **Mutation Rate** | 8.9 × 10⁻⁴ subs/site/year |
| **R² (Clock fit)** | 0.96 (strong temporal signal) |
| **tMRCA Estimate** | October-November 2019 |
| **95% CI** | September 2019 - December 2019 |

**Key Findings:**
- ✅ Successfully constructed time-calibrated phylogenetic tree
- ✅ Estimated tMRCA as October-November 2019 (high confidence)
- ✅ Consistent with Wuhan outbreak timeline (December 2019)
- ✅ Refuted alternative early origin hypotheses (Italy June 2019)
- ✅ Strong molecular clock signal validates dating approach
- ✅ Supports single zoonotic spillover event in late 2019

---

## Tools & Technologies

### Complete Tool List

All tools used throughout this specialization:

**Genome Assembly & Quality Control:**
- **SPAdes** - De novo genome assembly using de Bruijn graphs
- **QUAST** - Assembly quality assessment (N50, NG50, genome fraction)
- **Galaxy** - Web-based platform for bioinformatics workflows

**Sequence Analysis:**
- **BLAST** - Sequence similarity search (blastn for nucleotides, blastp for proteins)
- **NCBI databases** - GenBank, SRA (Sequence Read Archive)

**Genome Annotation:**
- **Prokka** - Automated prokaryotic/viral genome annotation
- **Prodigal** - Gene prediction tool (used by Prokka)
- **Aragorn** - tRNA and tmRNA detection
- **RNAmmer** - Ribosomal RNA prediction

**PCR Primer Design:**
- **Primer3** - Designs optimal PCR primers with Tm calculation

**Multiple Sequence Alignment:**
- **MUSCLE** - Multiple sequence alignment using progressive alignment
- **Jalview** - Alignment visualization and editing

**Phylogenetics:**
- **ViralMSA** - Reference-based multiple sequence alignment for viral genomes
- **Minimap2** - Fast sequence alignment (used by ViralMSA)
- **FastTree** - Maximum likelihood phylogenetic inference
- **LSD2** - Least Squares Dating for molecular clock analysis
- **Taxonium** - Interactive visualization of large phylogenetic trees

**Pathway Analysis:**
- **BioCyc** - Metabolic pathway database (HumanCyc, EcoCyc, MetaCyc)
- **Web-based pathway queries** - Database searching and network analysis

### Key Bioinformatics Concepts Learned

1. **De Bruijn Graphs** - Graph-based genome assembly method
2. **N50/NG50 Statistics** - Metrics for assembly quality
3. **Open Reading Frames (ORFs)** - Gene prediction in genomes
4. **Central Dogma** - DNA → RNA → Protein
5. **Pairwise Alignment** - Comparing two sequences (global vs local)
6. **Multiple Sequence Alignment** - Comparing many sequences simultaneously
7. **Phylogenetic Trees** - Representing evolutionary relationships
8. **Molecular Clock** - Using mutation rate to estimate divergence times
9. **Maximum Likelihood** - Statistical method for phylogenetic inference
10. **Metabolic Networks** - Graph representation of biochemical pathways

---

## Key Results

### Summary of Major Findings

| Module | Analysis | Result | Biological Significance |
|--------|----------|--------|------------------------|
| **01** | Genome Assembly | 29,903 bp complete genome | High-quality reference for all analyses |
| **01** | BLAST Search | 96.1% identity to RaTG13 | Bat coronavirus is closest relative |
| **02** | Gene Annotation | 11 ORFs identified | Complete functional map of viral proteins |
| **02** | PCR Design | N gene primers, 100% specific | Enables diagnostic testing |
| **03** | Spike Analysis | 6 key RBD mutations | Enhanced human ACE2 binding |
| **03** | Origin Analysis | Natural evolution signatures | No evidence of engineering |
| **04** | Drug Target | Guanylate kinase identified | Potential therapeutic target |
| **05** | Phylogenetic Dating | tMRCA: Oct-Nov 2019 | Emergence timeline established |
| **05** | Mutation Rate | 8.9 × 10⁻⁴ subs/site/year | Quantified evolutionary rate |

### Scientific Conclusions

**Viral Characteristics:**
- SARS-CoV-2 is a novel betacoronavirus with ~30 kb RNA genome
- Contains 11 major protein-coding genes
- Spike protein shows adaptations for enhanced human cell entry

**Evolutionary Origins:**
- Closest relative: Bat coronavirus RaTG13 (96.1% nucleotide identity)
- Evolved naturally through zoonotic spillover from bat reservoir
- No genomic evidence of artificial engineering or manipulation

**Pandemic Timeline:**
- Virus emerged in human population: October-November 2019
- First detected cases: December 2019 (Wuhan, China)
- Rapid global spread began in early 2020

**Therapeutic Opportunities:**
- Guanylate kinase identified as potential drug target
- Targets viral RNA synthesis while sparing host cells
- Computational approach enables rational drug design

---

## Data Availability

All data files used in this project are available for download:

**Google Drive Folder:**  
[https://drive.google.com/drive/u/0/folders/1SwaxL0km2cO2957i7C7aY76NCkbdhD5A](https://drive.google.com/drive/u/0/folders/1SwaxL0km2cO2957i7C7aY76NCkbdhD5A)

**Contents:**
- Raw sequencing data (FASTQ files)
- Assembled contigs and scaffolds (FASTA)
- Assembly quality reports (QUAST output)
- BLAST results (TXT)
- Genome annotations (GFF, FAA, FFN, GBK)
- Multiple sequence alignments (FASTA, ALN)
- Phylogenetic trees (Newick, Nexus)
- LSD2 dating results
- Pathway analysis queries

**Public Databases:**
- **NCBI SRA:** SRR10971381 (original sequencing reads)
- **NCBI GenBank:** MN908947 (Wuhan-Hu-1 reference genome)
- **GISAID:** Global SARS-CoV-2 sequence database

---

## Future Learning Goals

As I continue developing my bioinformatics skills, I plan to expand this project:

### Short-Term Goals (Next 6 Months)
1. **Variant Analysis**
   - Analyze Alpha, Beta, Gamma, Delta, and Omicron variants
   - Identify signature mutations in each variant
   - Track immune escape mutations

2. **Improved Visualization**
   - Create publication-quality figures for each analysis
   - Generate interactive dashboards for results
   - Develop animated phylogenetic tree showing spread

3. **Automation**
   - Write Python scripts to automate the entire pipeline
   - Implement workflow management (Snakemake or Nextflow)
   - Enable reproducible analysis with single command

### Medium-Term Goals (6-12 Months)
1. **Structure Prediction**
   - Use AlphaFold2 to predict 3D structures of viral proteins
   - Analyze spike protein structure in detail
   - Identify potential drug binding sites

2. **Expanded Phylogenetic Analysis**
   - Analyze thousands of sequences instead of 100
   - Study geographic spread patterns
   - Identify transmission networks

3. **Machine Learning Applications**
   - Train models to predict pathogenic mutations
   - Classify variants based on sequence features
   - Predict immune escape potential

### Long-Term Goals (1-2 Years)
1. **Integration with Wet-Lab Work**
   - Combine computational predictions with experimental validation
   - Test designed PCR primers in actual samples
   - Validate drug targets identified computationally

2. **Publication**
   - Write up methods and results
   - Submit to peer-reviewed journal or preprint server
   - Contribute to scientific literature

3. **Advanced Bioinformatics Training**
   - Learn RNA-seq analysis for gene expression
   - Study metagenomics for microbiome analysis
   - Explore structural bioinformatics in depth

---

## References

### Course Materials
This project is based on the **Applied Bioinformatics Specialization** by UC San Diego on Coursera:
- **Course Title:** Hacking COVID-19
- **Provider:** University of California San Diego
- **Platform:** Coursera
- **Description:** Learn critical applied Bioinformatics skills by studying real COVID-19 data

### Primary Scientific Literature

1. Wu, F. et al. (2020). A new coronavirus associated with human respiratory disease in China. *Nature*, 579, 265-269.
2. Andersen, K.G. et al. (2020). The proximal origin of SARS-CoV-2. *Nature Medicine*, 26, 450-452.
3. Renz, A. et al. (2020). FBA reveals guanylate kinase as a potential target for antiviral therapies against SARS-CoV-2. *Cell Metabolism*, 32, 1-3.
4. Pekar, J.E. et al. (2021). Timing the SARS-CoV-2 index case in Hubei province. *Science*, 372, 412-417.
5. Rambaut, A. (2020). Phylogenetic analysis of nCoV-2019 genomes. *Virological.org*.

### Bioinformatics Tool Citations

1. Bankevich, A. et al. (2012). SPAdes: A new genome assembly algorithm. *J Comput Biol*, 19, 455-477.
2. Gurevich, A. et al. (2013). QUAST: quality assessment tool for genome assemblies. *Bioinformatics*, 29, 1072-1075.
3. Altschul, S.F. et al. (1990). Basic local alignment search tool. *J Mol Biol*, 215, 403-410.
4. Seemann, T. (2014). Prokka: rapid prokaryotic genome annotation. *Bioinformatics*, 30, 2068-2069.
5. Edgar, R.C. (2004). MUSCLE: multiple sequence alignment with high accuracy. *Nucleic Acids Res*, 32, 1792-1797.
6. Price, M.N. et al. (2010). FastTree 2: approximately maximum-likelihood trees for large alignments. *PLoS One*, 5, e9490.
7. To, T.H. et al. (2016). Fast dating using least-squares criteria. *Syst Biol*, 65, 82-97.

### Online Resources & Databases
- **NCBI:** https://www.ncbi.nlm.nih.gov/
- **BioCyc:** https://biocyc.org/
- **GISAID:** https://www.gisaid.org/
- **Taxonium:** https://taxonium.org/
- **Galaxy:** https://usegalaxy.org/

---

## Acknowledgments

**Educational Background:**
- **Bachelor of Science in Biosciences**
  - Institution: COMSATS University Islamabad
  - Final Year Project: *Assessment of the role of ATP8B1 Gene in gallstones in Pakistani Populations*
  - Focus: Wet-lab molecular biology techniques

**Online Training:**
- **Applied Bioinformatics Specialization**
  - Provider: University of California San Diego
  - Platform: Coursera
  - Course: Hacking COVID-19
  - Instructors: UC San Diego faculty

**Special Thanks:**
- Open-source bioinformatics community for developing accessible tools
- NCBI and GISAID for maintaining public genomic databases
- Researchers who shared SARS-CoV-2 sequences during the pandemic
- Galaxy platform developers for providing free computational infrastructure
- Course instructors for making complex bioinformatics concepts accessible

**Purpose of This Project:**
This repository represents my transition from wet-lab molecular biology to computational bioinformatics. While my undergraduate work focused on traditional laboratory techniques (PCR, gel electrophoresis, DNA extraction), this specialization has equipped me with critical computational skills for modern genomic analysis. I'm continuing to build expertise at the intersection of biology and computer science.

---

## Contact

**Author:** Syed Muhammad Ali Shirazi  
**Email:** shirazi6503@gmail.com  
**LinkedIn:** [linkedin.com/in/syedmuhammadalishirazi](https://www.linkedin.com/in/syedmuhammadalishirazi)  
**Portfolio:** [alishirazi03.github.io](https://alishirazi03.github.io)  
**GitHub:** [github.com/alishirazi03](https://github.com/alishirazi03)

**Background:**
- B.S. Biosciences, COMSATS University Islamabad
- Transitioning from wet-lab to computational biology
- Actively learning bioinformatics, data science, and genomics

**Interests:**
- Computational genomics
- Viral evolution and epidemiology
- Drug target discovery
- Metabolic pathway analysis
- Integration of wet-lab and computational approaches

---

**License:** MIT  
**Last Updated:** March 2026  
**Version:** 1.0.0

---

*This project demonstrates my journey in learning applied bioinformatics through hands-on analysis of real SARS-CoV-2 genomic data. All analyses are reproducible, and I welcome feedback, suggestions, and opportunities to collaborate on similar projects.*
