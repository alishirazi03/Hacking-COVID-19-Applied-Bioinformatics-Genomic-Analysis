# SARS-CoV-2 Genomic Analysis Pipeline: From Assembly to Drug Target Discovery

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Bioinformatics](https://img.shields.io/badge/field-bioinformatics-green.svg)]()

## Project Overview

This repository contains a comprehensive bioinformatics analysis pipeline for SARS-CoV-2 genomic investigation, encompassing the complete workflow from raw sequencing data to evolutionary analysis and therapeutic target identification. The project demonstrates advanced computational biology techniques applied to real-world pandemic genomic data.

**Pipeline Scope:**
- ✅ De novo genome assembly from Illumina paired-end reads
- ✅ Functional genome annotation and gene prediction
- ✅ Comparative genomics and multiple sequence alignment
- ✅ Phylogenetic inference with temporal calibration
- ✅ Metabolic pathway reconstruction and drug target discovery

**Key Achievement:** Successfully assembled complete SARS-CoV-2 genome (29,903 bp), performed comprehensive annotation identifying 11 major ORFs, conducted phylogenetic analysis confirming zoonotic origin, and identified guanylate kinase as a high-priority antiviral drug target.

---

## Table of Contents
- [Biological Background](#background)
- [Pipeline Workflow](#workflow)
- [Course 1: Genome Assembly](#course1)
- [Course 2: Genome Annotation](#course2)
- [Course 3: Multiple Sequence Alignment & Origins](#course3)
- [Course 4: Metabolic Pathway Analysis](#course4)
- [Course 5: Phylogenetic Dating](#course5)
- [Technologies & Tools](#technologies)
- [Key Results](#results)
- [Installation & Usage](#installation)
- [Future Directions](#future)
- [References](#references)
- [Contact](#contact)

---

## Biological Background <a name="background"></a>

### SARS-CoV-2 Genomic Characteristics
- **Genome Type:** Positive-sense single-stranded RNA (+ssRNA)
- **Genome Length:** ~29.9 kb
- **Family:** Coronaviridae, Genus: Betacoronavirus
- **Closest Known Relative:** Bat coronavirus RaTG13 (96.1% nucleotide identity)
- **Key Proteins:** Spike (S), Nucleocapsid (N), Membrane (M), Envelope (E), RNA-dependent RNA polymerase (RdRp)

### Project Motivation
Understanding the SARS-CoV-2 genome is critical for:
1. **Diagnostic Development:** PCR primer design for rapid testing
2. **Therapeutic Discovery:** Identifying druggable targets
3. **Evolutionary Tracking:** Monitoring variant emergence and spread
4. **Pandemic Origins:** Establishing timeline and transmission pathways

---

## Pipeline Workflow <a name="workflow"></a>
