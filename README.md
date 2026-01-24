# TGF-β and IL-12 Conversely Orchestrate the Formation of CD103+ CD8 Tumor-Resident Memory T Cells to Regulate Response to Therapeutic Cancer Vaccine

## Bulk RNA-seq Analysis Project

---

## Table of Contents
- [Scientific Background](#scientific-background)
- [Project Motivation](#project-motivation)
- [Biological Context](#biological-context)
- [Technical Approach](#technical-approach)
- [Workflow](#workflow)
- [Challenges and Solutions](#challenges-and-solutions)
- [Tools & Technologies](#tools--technologies)
- [Key Learnings](#key-learnings)

---

## Scientific Background

### Research Question
**How do two immune signals (TGF-β and IL-12) control different types of CD8 T cells inside tumors, and how does that affect cancer immunotherapy?**

This study investigates the molecular mechanisms by which cytokines shape tumor-resident memory T cell (TRM) populations and influence therapeutic cancer vaccine efficacy.

---

## Project Motivation

I undertook this bulk RNA-seq analysis to:
- Gain hands-on experience with *Mus musculus* transcriptomic data
- Develop proficiency in Linux/Unix systems and bash scripting
- Build expertise with standard bioinformatics tools and workflows
- Apply computational skills to immunology research
- Understand how biological questions drive analytical approaches

---

## Biological Context

### The Main "Characters"

#### **CD8 T Cells**
- Cytotoxic immune cells that kill tumor cells
- Essential for cancer immunotherapy effectiveness

#### **TRM Cells (Tumor-Resident Memory T Cells)**
- CD8 T cells that permanently reside within tumors (don't recirculate)
- Their presence predicts better response to immunotherapy
- Critical for sustained tumor control

### Key Molecular Markers

These markers define the cell populations analyzed in the RNA-seq data:

| Marker | Population | Characteristics |
|--------|------------|-----------------|
| **CD103** | Stem-like TRM | Better tumor control, long-term immunity |
| **CD49a** | Effector TRM | More exhausted/effector-like, less stem-like |
| **Tcf-1** | Transcription factor | Associated with stem-like, self-renewing T cells |

### Two Major TRM Populations

**1. CD103+ TRM cells (often Tcf-1+)**
- More stem-like and self-renewing
- Associated with better long-term immunity
- Promoted by TGF-β

**2. CD49a+ TRM cells**
- More exhausted/effector-like
- Expanded after vaccination
- Promoted by IL-12

### The Cytokine Balance

**TGF-β:**
- Promotes CD103+ Tcf-1+ TRM cells
- Maintains stem-like characteristics
- Induces CD103 expression

**IL-12:**
- Opposes TGF-β effects
- Reduces CD103 expression
- Pushes cells toward exhaustion
- Suppresses Tcf-1

**Key Finding:** TGF-β and IL-12 act in opposite directions to orchestrate TRM differentiation

---

## Why This Matters for Cancer Immunotherapy

### Tumor-Infiltrating Lymphocytes (TILs)
- Evidence that the immune system recognizes the tumor
- More TILs → better response to immunotherapy
- Certain TRM subtypes are more effective than others

### Clinical Relevance
- PD-1 blocking antibodies (e.g., Keytruda) work best when CD103+ TRM cells are present
- Most tumors lack sufficient CD103+ TRM naturally
- Cancer vaccines shift the balance between stem-like and effector TRM populations

### The Knowledge Gap
1. How do TGF-β and IL-12 regulate TRM formation in the tumor microenvironment?
2. How does cancer vaccination change TRM populations?
3. Which TRM subsets are effector-like vs. stem-like?

---

## Experimental Design

### What the Researchers Did

1. **Mouse Model:** Melanoma tumors in mice (WT, CD103-KO, LSL-TβRICA transgenic)
2. **Cell Isolation:** Extracted immune cells from tumors
3. **FACS Sorting:** Isolated CD8 TILs into three populations:
   - CD103+ TILs
   - CD49a+ TILs
   - CD103⁻ CD49a⁻ TILs
4. **Bulk RNA-seq:** Each sample represents gene expression of a specific TIL population

### Key Findings
- Cancer peptide vaccine **reduces** Tcf-1+ CD103+ stem-like TRM
- Vaccine **increases** CD49a+ effector TRM (which control tumor growth)
- TGF-β and IL-12 inversely regulate CD103 and Tcf-1 expression

**Implication:** Controlling cytokines in tumors might enhance TRM differentiation and improve cancer immunotherapy outcomes.

---

## Technical Approach

### Initial Plan
I originally intended to work with:
- FastQC (quality control)
- STAR/HISAT2 (genome-based alignment)
- SAMtools (alignment processing)
- Snakemake (workflow management)

### Why I Changed Approaches

**Challenge:** Memory/RAM limitations on my Linux system caused frequent crashes during genome-based alignment with HISAT2.

**Solution:** Pivoted to **transcriptome-based quantification** using Salmon, which:
- Uses only exonic sequences (transcriptome) rather than the whole genome
- Employs lightweight pseudo-alignment
- Significantly reduces memory footprint
- Maintains analytical accuracy

---

## Workflow
```
FASTQ files (raw sequencing reads)
         ↓
    fastp (quality trimming & filtering)
         ↓
    Salmon (pseudo-alignment & quantification)
         ↓
    tximport (R: import transcript-level estimates)
         ↓
    DESeq2 (differential expression analysis)
```

### Pipeline Steps

1. **Quality Control:** FastQC to assess read quality
2. **Trimming:** fastp to remove adapters and low-quality bases
3. **Quantification:** Salmon for transcript abundance estimation
4. **Import:** tximport to aggregate transcript-level counts to gene-level
5. **Differential Expression:** DESeq2 to identify genes distinguishing TRM populations

### Automation
- Implemented bash loops for batch processing of trimming and quantification
- Processed multiple samples efficiently

---

## Challenges and Solutions

| Challenge | Solution |
|-----------|----------|
| **RAM limitations** with genome alignment | Switched to Salmon (transcriptome-based) |
| **Downloading reference files** (genome/transcriptome) | Used Ensembl FTP for *Mus musculus* transcriptome |
| **HISAT2 crashes** | Replaced with memory-efficient Salmon |
| **Understanding biological context** | Read entire paper before analysis to ensure directional, meaningful conclusions |

---

## Tools & Technologies

### Bioinformatics Tools
- **Quality Control:** FastQC, fastp
- **Quantification:** Salmon (v1.x)
- **Statistical Analysis:** R (tximport, DESeq2)

### Reference Data
- **Species:** *Mus musculus* (mouse)
- **Transcriptome:** Ensembl/GENCODE

### Programming & Environment
- **Scripting:** Bash
- **Operating System:** Linux/Unix
- **Language:** R (for statistical analysis)

### Data Structure
- **GSE** = Study (Gene Expression Omnibus series)
- **GSM** = Individual samples
- **SRR** = Sequencing runs (FASTQ files)

---

## Key Learnings

### Technical Skills
- Hands-on experience with bulk RNA-seq analysis pipeline
- Proficiency in Linux/Unix command-line operations
- Workflow optimization under computational constraints
- Integration of multiple bioinformatics tools

### Biological Insights
- Understanding how cytokine signaling shapes immune cell differentiation
- Connecting molecular markers (CD103, CD49a, Tcf-1) to functional cell states
- Appreciating the complexity of tumor-immune interactions
- Recognizing how biological questions guide analytical choices

### Problem-Solving
- Adapting workflows when faced with resource limitations
- Evaluating trade-offs between different analytical approaches
- Importance of understanding the science before analyzing data

---

## TL;DR

1. **Tumors need resident CD8 T cells (TRM) to respond to immunotherapy**
2. **TRM cells are marked by CD103 and influenced by Tcf-1**
3. **TGF-β (promotes TRM) and IL-12 (inhibits TRM) control differentiation**
4. **Cancer vaccines shift balance between stem-like (Tcf-1+) and effector (CD49a+) TRM**
5. **This analysis decodes molecular signals to suggest therapy improvements**

---

## Future Directions
- Implement full differential expression analysis with DESeq2
- Perform gene set enrichment analysis (GSEA) on TRM signatures
- Explore transcription factor regulatory networks
- Compare findings with published results

---

## Repository Structure
```
.
├── data/
│   ├── raw/                # Raw FASTQ files
│   ├── trimmed/            # Quality-trimmed reads
│   └── quantification/     # Salmon output
├── scripts/
│   ├── 01_download_data.sh
│   ├── 02_fastqc.sh
│   ├── 03_fastp_trim.sh
│   ├── 04_salmon_quant.sh
│   └── 05_deseq2_analysis.R
├── results/
│   ├── qc_reports/
│   ├── counts/
│   └── differential_expression/
├── references/
│   └── mus_musculus_transcriptome/
└── README.md
```

---

## Getting Started

### Prerequisites
```bash
# Install required tools
conda install -c bioconda fastqc fastp salmon
conda install -c conda-forge r-base
```

### R Packages
```r
install.packages("BiocManager")
BiocManager::install(c("tximport", "DESeq2", "AnnotationDbi"))
```

### Running the Pipeline

1. **Download reference transcriptome:**
```bash
bash scripts/01_download_data.sh
```

2. **Quality control:**
```bash
bash scripts/02_fastqc.sh
```

3. **Trim reads:**
```bash
bash scripts/03_fastp_trim.sh
```

4. #### Run Salmon Quantification:
```bash
# Loop through all SRR accessions and quantify
while read SRR; do
    if [ ! -d salmon/${SRR} ]; then
        echo "Running Salmon for $SRR"
        salmon quant \
            -i reference/salmon_index \
            -l A \
            -1 trimmed/${SRR}_1.trimmed.fastq.gz \
            -2 trimmed/${SRR}_2.trimmed.fastq.gz \
            -p 2 \
            --validateMappings \
            -o salmon/${SRR}
    else
        echo "Salmon already done for $SRR"
    fi
    echo "Finished $SRR"
    echo
done < metadata/srr_list.txt
```

**Salmon Parameters Explained:**
- `-i`: Path to Salmon index (transcriptome)
- `-l A`: Automatically detect library type
- `-1` / `-2`: Forward and reverse paired-end reads
- `-p 2`: Number of threads (adjust based on available CPU)
- `--validateMappings`: More accurate quantification (slightly slower)
- `-o`: Output directory for this sample

**Example `metadata/srr_list.txt` format:**
```
SRR1234567
SRR1234568
SRR1234569
```

5. **Differential expression analysis:**
```r
Rscript scripts/05_deseq2_analysis.R
```

---

## Citation

**Original Paper:**
Corgnac S, Damei I, Gentile C, Caidi A, Badel S, Phayanouvong M, Mami-Chouaib F. TGF-β and IL-12 conversely orchestrate the formation of CD103+ CD8 tumor-resident memory T cells to regulate response to therapeutic cancer vaccine. iScience. 2025 Jul 18;28(8):113147. doi: 10.1016/j.isci.2025.113147. PMID: 40799399; PMCID: PMC12341530.

**Data Source:**
GEO Accession: GSE298151

---

## License

This project is for educational purposes.

---

## Contact

**Author:** Mahalakshmi  
**Project:** Bulk RNA-seq Analysis - TRM Cell Differentiation

---

**Note:** This is an independent learning project based on publicly available data from the cited publication.
