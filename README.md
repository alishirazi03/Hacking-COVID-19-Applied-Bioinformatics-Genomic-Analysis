# Hacking COVID-19: Applied Bioinformatics & Genomic Analysis

## 📌 Project Overview
This repository contains a comprehensive bioinformatics pipeline developed to analyze SARS-CoV-2 genomic data. Based on real-world datasets from the COVID-19 pandemic, this project demonstrates the end-to-end process of viral genomic analysis—from raw read assembly to the identification of potential metabolic drug targets. 

## 🛠️ Skills & Technologies
* **Domain Knowledge:** Computational Biology, Genomics, Metabolic Pathway Analysis, Phylogenetics.
* **Bioinformatics Techniques:** Genome Assembly, Sequence Annotation, Multiple Sequence Alignment (MSA), Maximum-Likelihood Phylogenetic Inference, Molecular Clock Dating.
* **Data Formats Handled:** FASTA, FASTQ, GFF, NEXUS, NHX, NWK.

## 📂 Repository Structure

### `01_Genome_Assembly`
Contains the foundational step of the pipeline: assembling the SARS-CoV-2 genome from raw sequencing data. 
* **Key Files:** Assembled `.fasta` genomes, `.contigs`, and `.scaffolds`.
* **Objective:** Demonstrates quality control and the reconstruction of a viral genome using state-of-the-art assembly algorithms.

### `02_Genome_Annotation`
Focuses on identifying the functional elements within the assembled genome to design targeted diagnostic tests.
* **Key Files:** Annotation files (e.g., `.gff`, `.gbk`).
* **Objective:** Showcases the ability to map genes, open reading frames (ORFs), and functional regions of the pathogen.

### `03_Evolution_and_Phylogenetics`
Investigates the origins and evolutionary trajectory of the virus using genomic dating and tree-building.
* **Key Files:** Multiple Sequence Alignments (`.aln`, `.fas`), Phylogenetic trees (`.nhx`, `.nexus`), and MRCA dating data.
* **Objective:** Illustrates the application of maximum-likelihood inference and molecular clock analysis to trace the virus's spread over time.

### `04_Metabolic_Pathways`
Explores the intersection of the virus and human host metabolism to identify potential therapeutic vulnerabilities.
* **Key Files:** Pathway visualization PDFs (e.g., Human Metabolic Network) and Chokepoint Reaction Reports.
* **Objective:** Highlights advanced computational drug discovery techniques, specifically utilizing reachability analysis to pinpoint essential "chokepoint" reactions for potential drug targeting.

## 🚀 Conclusion
This portfolio project reflects practical, applied experience in tackling a real-world biological crisis using modern computational tools, bridging the gap between raw sequencing data and actionable medical insights.
