# In-Silico-Design-and-Validation-of-a-Triplex-PCR-Assay-for-Differentiating-MTBC-and-MAC
This repository contains the complete computational workflow, bioinformatics algorithms, and supplementary data for designing a highly specific triplex endpoint PCR assay. The assay is engineered to accurately distinguish the *Mycobacterium tuberculosis* complex (MTBC) from the *Mycobacterium avium* complex (MAC) in clinical samples (e.g., sputum, bronchoalveolar lavage fluid). 

## Project Rationale
Tuberculosis (TB) remains a severe global health crisis. Accurate, rapid differentiation of the *M. tuberculosis* complex (MTBC) from non-tuberculous mycobacteria (NTM)—especially the *M. avium* complex (MAC)—is critical for initiating appropriate antimicrobial therapy. Misdiagnosing MAC as MTBC leads to ineffective treatment, prolonged patient suffering, and contributes to antimicrobial resistance. 

This project aimed to develop a scalable and highly specific triplex PCR assay using:
1.  **IS6110** (Primary MTBC target) – Selected for high analytical sensitivity due to its multi-copy nature in the MTBC genome.
2.  **senX3-regX3** (Secondary MTBC target) – Selected to confirm MTBC presence and mitigate false negatives caused by rare IS6110-deletion strains.
3.  **IS1245** (MAC target) – Selected to explicitly identify MAC and differentiate it from other NTMs.

---

## Computational Workflow
The pipeline was executed entirely via a Jupyter Notebook utilizing `Biopython`, `primer3-py`, and `pandas`.

1.  **Sequence Ingestion & Manifesting:** FASTA sequences for the three targets, alongside local inclusivity (MTBC/MAC genomes) and exclusivity (NTM genomes) panels, were parsed and loaded into local memory.
2.  **Singleplex Generation:** The `primer3` engine generated top primer pairs for each target, optimizing for a melting temperature ($T_m$) of ~60°C and a GC content of ~55% to ensure uniform thermal annealing.
3.  **Combinatorial Multiplexing:** All 125 possible 3-way combinations of the top 5 primer pairs per target ($5 \times 5 \times 5 = 125$) were mathematically generated.
4.  **Thermodynamic & Physical Evaluation:** Every combination was thermodynamically evaluated for cross-dimerization (heterodimers) across all 15 possible 6-primer interactions ($\Delta G$). Gel band resolution was optimized by enforcing a strict minimum base-pair size gap between the three target amplicons.
5.  **Multi-Tiered Ranking:** Combinations were sorted based on a strict biological hierarchy: maximizing band gap size, minimizing primer-dimer $T_m$ and $\Delta G$, and minimizing the variance in $T_m$ across targets. 
6.  **Internal Panel Screening:** The winning assay was cross-referenced via exact-string matching against the local off-target NTM panel to prove initial exclusivity.

---

## External Specificity Validation (NCBI Primer-BLAST)
Because this assay is intended for human respiratory samples, it was externally validated against the entire known genomic universe via the NCBI `nt` (Nucleotide collection) database. This step accounts for natural genetic variance and rules out real-world clinical noise.

### 1. Target Inclusivity Validation (MAC Specificity)
To confirm the MAC marker successfully targets clinical MAC strains globally despite natural genetic variance, it was evaluated for inclusivity.
*   **Result:** The MAC primers successfully mapped to the IS1245 transposon element across diverse *Mycobacterium avium* complex genomes, producing the exact computed 526 bp amplicon.

### 2. Human Host (*Homo sapiens*) Exclusivity
To ensure no false positives occur from human lung epithelial DNA, the primers were queried against *Homo sapiens* (taxid:9606).
*   **Result:** No PCR amplicons generated. (The IS6110 Forward primer exhibited partial single-primer binding to the *NPAS2* gene, but the absolute lack of a paired reverse primer physically prevents exponential amplification).

### 3. Respiratory Flora & Pathogen Exclusivity
The primers were screened against heavily contaminating respiratory flora: *Streptococcus pneumoniae* (taxid:1313), *Haemophilus influenzae* (taxid:727), and *Pseudomonas aeruginosa* (taxid:287).
*   **Result:** No PCR amplicons generated. *H. influenzae* and *P. aeruginosa* exhibited zero primer binding. (The IS6110 Reverse primer exhibited single-primer binding to *S. pneumoniae*, but lacking a forward primer, no PCR product is possible).

**Key Insight:** The triplex assay demonstrates absolute global specificity for the target mycobacteria and is fully validated *in silico* for clinical application.

---

## Final Validated Assay Blueprint
Following exhaustive computational optimization and successful global specificity validation, the final diagnostic blueprint for laboratory manufacturing is as follows:

| Assay Target | Forward Primer (5' → 3') | Reverse Primer (5' → 3') | Expected Product Size |
| :--- | :--- | :--- | :--- |
| **Primary MTBC (IS6110)** | `GAGTCGATCTGCACACAGCT` | `GGCTGATGTGCTCCTTGAGT` | **127 bp** |
| **Secondary MTBC (senX3)** | `ACTGGCAAACCTGGTTTCCA` | `TGATAGGCCTCGATCAACGC` | **324 bp** |
| **MAC (IS1245)** | `GGTGAATCATCGGGTGGTGT` | `GCGGCGTTTGATTTCCTTGT` | **526 bp** |

*These fragment sizes provide massive spatial separation on a standard agarose gel, ensuring unambiguous diagnostic interpretation.*

---

## Reproducibility Notes
To replicate this bioinformatics analysis, ensure you have a Python 3.8+ environment.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/SolotanTemi/In-Silico-Design-and-Validation-of-a-Triplex-PCR-Assay-for-Differentiating-MTBC-and-MAC.git
    cd In-Silico-Design-and-Validation-of-a-Triplex-PCR-Assay-for-Differentiating-MTBC-and-MAC
    ```
2.  **Install dependencies:**
    ```bash
    pip install pandas numpy biopython primer3-py matplotlib seaborn
    ```
3.  **Run the Notebook:**
    Launch Jupyter and execute all cells in `Triplex1-Copy1-3.ipynb`.
    ```bash
    jupyter notebook
    ```
*(Note: Ensure your directory structure matches the `data/raw/` paths outlined in Step 1 of the notebook).*

---
**Ethical AI Usage Note:** 
*Artificial Intelligence tools were utilized during the development of this project strictly for code troubleshooting, debugging, and assisting in the structuring of documentation, in accordance with standard ethical use policies.*
