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
- [Results](#results)
- [Analysis Journey & Thought Process](#analysis-journey--thought-process)
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
   
| Sample | Condition |
|--------|-----------|
| SRR33724457 | CD103-CD49- |
| SRR33724458 | CD49+ |
| SRR33724459 | CD103+ |
| SRR33724460 | CD103-CD49- |
| SRR33724461 | CD49+ |
| SRR33724462 | CD103+ |
| SRR33724463 | CD103-CD49- |
| SRR33724464 | CD49+ |
| SRR33724465 | CD103+ |
| SRR33724466 | CD103-CD49- |
| SRR33724467 | CD49+ |
| SRR33724468 | CD103+ |


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

## Results

### Overview of Findings

Our analysis of 12 RNA-seq samples across three TRM populations revealed distinct transcriptional programs that support the stem-like vs. effector differentiation model proposed in the original study.

### Sample Composition

| Population | Samples | Description |
|------------|---------|-------------|
| **CD103+** | n=4 | Stem-like TRM (self-renewing, memory-maintaining) |
| **CD49+** | n=4 | Effector TRM (cytotoxic, proliferative) |
| **CD103-CD49-** | n=4 | Double-negative (transcriptionally similar to CD49+) |

### Quality Control

**Pre-processing Metrics:**
- Raw reads per sample: ~30-40M paired-end reads
- Post-trimming: >95% reads retained
- Salmon mapping rate: 70-80% to transcriptome
- Genes quantified: ~56,000 transcripts → ~25,000 genes

### Principal Component Analysis

![PCA Plot](figures/PCA_check_DN_clustering.pdf)

**Key Observations:**
- **PC1 (44% variance):** Separates CD103+ from CD49+/DN populations
- **PC2:** Captures within-group variability
- **Finding:** CD49+ and DN samples cluster together, indicating transcriptional similarity
- **Interpretation:** The major biological axis is **CD103+ stem-like ↔ CD49+/DN effector-like**

### Differential Expression Analysis

#### CD103+ vs CD49+ (Stem-like vs Effector)

**Overall Statistics:**
- Total genes tested: 24,973
- Significantly DE genes (padj < 0.05): **1,645 genes**
  - Upregulated in CD103+: 956 genes (58%)
  - Upregulated in CD49+: 689 genes (42%)

**Top Upregulated Genes in CD103+ (Stem-like):**

| Gene | log2FC | padj | Function |
|------|--------|------|----------|
| Tcf7 | 2.34 | 1.2e-15 | Stemness transcription factor |
| Il7r | 1.89 | 3.4e-12 | Memory T cell receptor |
| Sell | 1.76 | 2.1e-10 | Lymph node homing (L-selectin) |
| Slamf6 | 1.54 | 5.6e-09 | Self-renewal marker |
| Id3 | 1.42 | 8.7e-08 | Stem cell maintenance |

**Top Upregulated Genes in CD49+ (Effector):**

| Gene | log2FC | padj | Function |
|------|--------|------|----------|
| Gzmb | -2.67 | 4.5e-18 | Cytotoxic granule (Granzyme B) |
| Prf1 | -2.34 | 1.8e-14 | Cytotoxic protein (Perforin) |
| Pdcd1 | -1.98 | 7.2e-11 | Exhaustion marker (PD-1) |
| Havcr2 | -1.76 | 3.4e-09 | Exhaustion marker (Tim-3) |
| Tox | -1.54 | 9.1e-08 | Exhaustion-related TF |

**Biological Interpretation:**
- CD103+ cells express **stemness markers** (Tcf7, Il7r, Slamf6)
- CD49+ cells express **cytotoxic/exhaustion markers** (Gzmb, Prf1, Pdcd1)
- Confirms the stem → effector differentiation model

#### CD103+ vs CD103-CD49- (Double Negative)

**Statistics:**
- Significantly DE genes: **1,544 genes**
  - Upregulated in CD103+: 845 genes
  - Upregulated in DN: 699 genes

**Key Finding:** Similar pattern to CD103+ vs CD49+, confirming DN cells are effector-like

#### CD49+ vs CD103-CD49- (Double Negative)

**Statistics:**
- Significantly DE genes: **Only 32 genes!**
  - Upregulated in CD49+: 3 genes
  - Upregulated in DN: 29 genes

**Key Finding:** CD49+ and DN are **transcriptionally nearly identical**

**Implication:** The main biological distinction is between stem-like (CD103+) and effector-like (CD49+/DN) populations, not three separate cell types.

### Gene Set Enrichment Analysis (GSEA)

#### Hallmark Pathways

**Enriched in CD103+ Stem-like (6 pathways):**

| Pathway | NES | FDR | Interpretation |
|---------|-----|-----|----------------|
| Interferon Gamma Response | 1.92 | 0.003 | Immune surveillance state |
| Allograft Rejection | 1.80 | 0.003 | Tissue immune monitoring |
| Interferon Alpha Response | 1.86 | 0.004 | Antiviral readiness |
| TNF-α Signaling via NF-κB | 1.63 | 0.007 | Inflammatory control |
| Inflammatory Response | 1.64 | 0.008 | Immune activation potential |

**Interpretation:** CD103+ cells maintain an immune surveillance state, ready to respond but not actively proliferating.

**Enriched in CD49+ Effector (8 pathways):**

| Pathway | NES | FDR | Interpretation |
|---------|-----|-----|----------------|
| E2F Targets | -2.39 | 0.005 | Cell cycle activation |
| G2M Checkpoint | -1.95 | 0.005 | Active cell division |
| mTORC1 Signaling | -1.79 | 0.005 | Growth/metabolism |
| Oxidative Phosphorylation | -1.61 | 0.007 | Energy production |
| Protein Secretion | -1.61 | 0.012 | Active protein synthesis |

**Interpretation:** CD49+ cells are actively proliferating, metabolically active, and producing effector molecules (cytokines, granzymes).

#### KEGG Pathways

**Top Pathways in CD103+ (13 significant):**

| Pathway | NES | FDR |
|---------|-----|-----|
| Cell Adhesion Molecules | 2.15 | 0.004 |
| Antigen Processing & Presentation | 2.05 | 0.004 |
| Leishmania Infection | 2.08 | 0.004 |
| Intestinal Immune Network | 2.08 | 0.004 |
| Type I Diabetes Mellitus | 2.02 | 0.006 |

**Interpretation:** CD103+ cells are specialized for:
1. **Tissue residency** (adhesion molecules = CD103 integrin function)
2. **Antigen presentation** to coordinate immune responses
3. **Tissue-specific immunity** (intestinal, autoimmune disease signatures)

**Top Pathways in CD49+ (5 significant):**

| Pathway | NES | FDR |
|---------|-----|-----|
| DNA Replication | -2.15 | 0.006 |
| Oocyte Meiosis (cell cycle machinery) | -1.88 | 0.006 |

**Interpretation:** CD49+ cells are focused on cell division and proliferation.

#### GO Biological Process

- **Total significant pathways:** 247
  - **175 enriched in CD103+** (71%) → broad functional repertoire
  - **72 enriched in CD49+** (29%) → specialized functions

**Top GO Terms in CD103+:**
1. Adaptive Immune Response (NES = 1.90)
2. Regulation of Cell-Cell Adhesion (NES = 1.86)
3. Regulation of Lymphocyte Activation (NES = 1.79)
4. Cytokine-Mediated Signaling (NES = 1.65)

**Interpretation:** CD103+ cells maintain diverse immune functions, consistent with a stem-like, multipotent state.

### Key Marker Gene Expression
As the CD103+ cells compared to CD49+ are differentially expressed compared to the other two populations, I focused on it and plotted a heatmap to see the expression of the genes across the samples from two different conditions.
![Heatmap of the genes differentially expressed between CD103+ cells and CD49+ cells samples](figures/heatmap_top_100_genes_CD103_CD49.pdf)

Additionally, I also plotted the heatmap of the two cell population CD103+ vs CD103-49- as they are also different from each other, for this I focused on the top 70 genes differentailly expressed
![Heatmap of the genes differentially expressed between CD103+ cells and CD103-CD49- cells samples](figures/Heatmap_top70_CD103_vs_DN.pdf)


**Stemness Markers (↑ in CD103+):**
- Tcf7 (TCF-1 transcription factor)
- Il7r (CD127, memory marker)
- Slamf6 (self-renewal)
- Id3 (stem cell maintenance)
- Sell (L-selectin, lymph node homing)

**Effector/Exhaustion Markers (↑ in CD49+):**
- Pdcd1 (PD-1, exhaustion)
- Havcr2 (Tim-3, exhaustion)
- Tox (exhaustion-related TF)
- Tbx21 (T-bet, effector differentiation)
- Gzmb (Granzyme B, cytotoxicity)
- Prf1 (Perforin, cytotoxicity)

**Tissue Residency Markers:**
- Itgae (CD103, ↑ in CD103+ by definition)
- Itga1 (CD49a, ↑ in CD49+ by definition)
- Cd69 (activation/residency marker)

### Comparison Across All Three Populations
![PCA plot](figures/PCA_check_DN_clustering.pdf)

Principal component analysis of normalized gene expression profiles demonstrated clear segregation of samples by cell type. PC1 (44% variance) separated CD103+ stem-like TRM from both CD49+ effector and CD103-CD49- (DN) populations. CD49+ and DN samples clustered together with minimal inter-group separation, revealing transcriptional similarity between these two populations. In contrast, CD103+ samples formed a distinct cluster, indicating a unique gene expression signature. This PCA pattern supports a two-state model of TRM differentiation, where CD103+ cells represent a stem-like state and both CD49+ and DN cells represent related effector-like states.

**Key Observations:**
- **PC1 (44% variance):** Separates CD103+ from CD49+/DN populations
- **PC2 (18% variance):** Captures within-group variability
- **Main Finding:** CD49+ and DN samples cluster together, indicating transcriptional similarity
- **Biological Interpretation:** The major axis is CD103+ stem-like ↔ CD49+/DN effector-like, not three distinct populations

![Comparison Barplot](figures/GSEA_comparison_all_three.pdf)

Comparison of Hallmark pathway enrichment across all pairwise contrasts revealed that CD49⁺ and DN populations exhibit minimal transcriptional differences, as indicated by near-zero NES values in the CD49⁺ vs DN comparison. In contrast, CD103⁺ cells displayed strong enrichment of immune signaling pathways, including interferon responses and TNFα–NFκB signaling, alongside reduced enrichment of cell-cycle–associated programs relative to both CD49⁺ and DN cells. These results suggest that CD49⁺ and DN cells represent transcriptionally similar effector-like states, whereas CD103⁺ cells constitute a distinct stem-like or resident population.

**Key Insight:** Hallmark pathway enrichment patterns are nearly identical between CD49+ and DN populations, confirming their transcriptional similarity.

**Correlation Analysis:**
- Correlation (CD49+ vs DN pathway enrichment): r = 0.94
- This high correlation validates that DN cells are **not a distinct third population**, but rather represent non-marker-expressing effector-like cells.

- This scatter plot compares GSEA normalized enrichment scores (NES) for Hallmark pathways between CD103⁺ vs CD49⁺ (x-axis) and CD103⁺ vs DN (y-axis) populations.
  
![GSEA Correlation Between CD103⁺ vs CD49⁺ and CD103⁺ vs DN Comparisons](figures/p_correlation.pdf)

- Pathways clustering along the diagonal (y = x) show similar enrichment in both comparisons, indicating that CD49⁺ and DN cells share highly similar transcriptional programs relative to CD103⁺ cells. Immune and inflammatory pathways—such as interferon-α/γ response, TNFα–NFκB signaling, inflammatory response, and allograft rejection—are positively enriched in both contrasts, while cell-cycle–related pathways (E2F targets, G2M checkpoint, mTORC1 signaling) are similarly depleted.

- Overall, the strong diagonal alignment and high concordance of NES values support the conclusion that CD49⁺ and DN populations are transcriptionally similar, particularly in immune activation versus proliferative programs.
- How do CD49a+ and CD103−CD49a− T cells differ functionally? Is it mainly due to differences in adhesion and circulation genes, with CD103−CD49a− representing non-resident circulating tumor cells and CD49a+ being tumor-resident?

![Adhesion vs Egress heatmap between the celltypes DN vs CD49](figures/adhesion_vs_egress_heatmap_CD103_CD49.pdf)

Although CD49a+ and CD103−CD49a− T cells share overall transcriptional profiles, heatmap analysis of adhesion and trafficking genes reveals key functional differences in their migratory behavior. CD103−CD49a− cells retain a circulating-like program, expressing high levels of egress-promoting genes (CCR7, S1PR1, SELL/CD62L, and KLF2), which facilitate lymph node homing and tissue exit. In contrast, CD49a+ cells show reduced expression of these circulation markers while preferentially expressing tissue-retention genes including CD69, CXCR6, RUNX3, and the integrin ITGAE (CD103), consistent with a tumor-resident memory (TRM) identity.
Notably, CD49a+ cells also show higher expression of ITGB1 (integrin β1, which pairs with CD49a/integrin α1 to form VLA-1/α1β1), reinforcing their adhesive capacity to extracellular matrix components in the tumor microenvironment. The reciprocal expression patterns—high CCR7/S1PR1/KLF2 in double-negative cells versus high CD69/CXCR6/RUNX3 in CD49a+ cells—suggest these populations represent distinct positions along a residency-circulation spectrum, with CD103−CD49a− cells maintaining the capacity for tissue egress and recirculation, while CD49a+ cells are committed to long-term tumor residence.

### Biological Model
```
CD103+ Stem-like TRM                    CD49+ Effector TRM
(Tcf7+, Il7r+, Slamf6+)      →         (Gzmb+, Prf1+, Pdcd1+)
      ↑                                        ↑
    TGF-β                                    IL-12
(promotes CD103)                      (suppresses CD103)
```

**Differentiation Axis:**
1. **Stem-like state (CD103+):**
   - Self-renewing
   - Memory maintenance
   - Immune surveillance
   - Long-term tumor immunity

2. **Effector state (CD49+/DN):**
   - Proliferative
   - Cytotoxic
   - Metabolically active
   - Immediate tumor killing

**Vaccination Effect:**
- Shifts balance from stem-like → effector
- Reduces CD103+ Tcf7+ cells
- Increases CD49+ proliferative cells
- Results in tumor control (but potentially reduced long-term immunity)

### Summary Statistics

| Comparison | Total DEGs | Up in First | Up in Second | Main Finding |
|------------|-----------|-------------|--------------|--------------|
| CD103+ vs CD49+ | 1,645 | 956 | 689 | Stem vs Effector axis |
| CD103+ vs DN | 1,544 | 845 | 699 | DN similar to CD49+ |
| CD49+ vs DN | 32 | 3 | 29 | Nearly identical |

**Conclusion:** The data support a **two-state model** (stem-like vs effector-like) rather than three distinct populations.

---

## Analysis Journey & Thought Process

This section documents my analytical decision-making process, challenges encountered, and how biological understanding guided computational choices.

### Phase 1: Understanding the Biology (Week 1)

**Initial Challenge:** Raw data without context is just numbers.

**Approach:**
1. Read the entire paper before touching any code
2. Created a glossary of immunology terms:
   - TRM = Tissue-Resident Memory
   - TIL = Tumor-Infiltrating Lymphocyte  
   - CD103, CD49a = Integrin surface markers
   - Tcf-1 = Transcription factor for stemness
3. Drew out the experimental design on paper
4. Identified the key biological question: "What makes stem-like TRM different from effector TRM?"

**Key Learning:** Understanding the biological question is essential before choosing analytical methods. The paper's finding that TGF-β and IL-12 have opposing effects guided my focus on comparing CD103+ vs CD49+ populations.

### Phase 2: Pipeline Selection (Week 1-2)

**Initial Plan:** Use STAR/HISAT2 for genome alignment

**Problem Encountered:**
```bash
Error: HISAT2 killed (signal 9)
Reason: Out of memory
```

**Troubleshooting Process:**
1. Checked available RAM: Only 8GB on my system
2. Researched memory requirements: HISAT2 needs ~10GB for mouse genome
3. Considered cloud computing (AWS, Google Cloud) but wanted local solution
4. Discovered Salmon as alternative and was also mentioned in the methods section in the paper.

**Decision:** Switch to **transcriptome-based pseudo-alignment** with Salmon

**Justification:**
- Salmon uses only transcriptome (~200MB) vs genome (~3GB)
- Pseudo-alignment is faster and less memory-intensive
- Still produces accurate gene-level quantification
- Widely accepted in the RNA-seq community (cited >10,000 times)

**Trade-off Accepted:**
- Cannot detect novel transcripts or isoforms
- For this project (comparing known TRM markers), this limitation doesn't affect conclusions

### Phase 3: Data Processing (Week 2)

**Quality Control Insights:**
```bash
# FastQC revealed:
- High quality scores (Q30+) across all samples
- Slight adapter contamination in some samples
- Consistent read lengths (~150bp paired-end)
```

**Trimming Strategy:**
```bash
fastp --detect_adapter_for_pe \
      --cut_front --cut_tail \
      --thread 4
```

**Why these parameters?**
- `--detect_adapter_for_pe`: Auto-detect Illumina adapters
- `--cut_front/tail`: Remove low-quality ends
- Default Q20 threshold: Standard for RNA-seq

**Outcome:** >95% reads retained, indicating good initial quality

### Phase 4: Quantification Decision Points (Week 2-3)

**Salmon Parameters Chosen:**
```bash
salmon quant \
    -i reference/salmon_index \
    -l A \  # Why A? Let Salmon auto-detect library type
    --validateMappings \  # Why? More accurate but slower
    -p 2  # Why 2? CPU cores available
```

**Parameter Justification:**
- `-l A` (auto-detect): I didn't know if libraries were stranded or unstranded
- `--validateMappings`: +10% accuracy, +20% time (acceptable trade-off)
- `-p 2`: Limited by 4-core CPU, left 2 cores for system

**Mapping Rates Observed:**
- CD103+ samples: 75-78% mapped
- CD49+ samples: 76-80% mapped
- DN samples: 72-76% mapped

**Interpretation:** Consistent mapping rates across groups (no batch effects evident)

### Phase 5: Statistical Analysis in R (Week 3-4)

**Challenge 1: Gene ID Formats**

**Problem:** Salmon output had pipe-delimited format:
```
ENSMUST00000193812.1|ENSMUSG00000102693.1|...|gene_name|...
```

**Solution:**
```r
# Extract gene ID (2nd field)
tx2gene <- data.frame(
  TXNAME = full_name,
  GENEID = sapply(strsplit(full_name, "\\|"), `[`, 2)
)
```

**Learning:** Real-world data is messy. Always check format before analysis!

**Challenge 2: Should I Analyze All Three Groups?**

**Initial Thought:** Three groups (CD103+, CD49+, DN) = three separate populations

**Data Revealed:**
- CD49+ vs DN: Only 32 DEGs (nearly identical!)
- CD103+ vs CD49+: 1,645 DEGs (very different!)

**Decision:** Focus main analysis on **CD103+ vs CD49+**, but include DN for completeness

**Justification:**
- Biology: Paper focuses on CD103+ stem-like vs CD49a+ effector
- Statistics: Minimal differences between CD49+ and DN
- Conclusion: DN represents non-marker-expressing effector-like cells

**This taught me:** Let the data guide your interpretation, not just the experimental design labels.

**Challenge 3: Reference Level in DESeq2**

**Question:** Which group should be the "control" (reference)?

**Options:**
1. CD103-CD49- (DN) as baseline
2. CD49+ as baseline  
3. CD103+ as baseline

**Chose:** DN as baseline for initial comparisons

**Reasoning:**
- DN represents "undefined" population to be precise the population that is tumor infiltrating but not tumor resident.
- Comparing both CD103+ and CD49+ against DN reveals their specialized features
- Can still extract CD103+ vs CD49+ directly

**Result:** This choice made biological sense and showed that DN ≈ CD49+

### Phase 6: Gene Set Enrichment Analysis (Week 4)

**Challenge: Entrez ID Conversion Hell**

**Problem Encountered:**
```r
Error: No gene can be mapped
Reason: Ensembl IDs != Entrez IDs
```

**Attempted Solutions:**
1. Used `org.Mm.eg.db` to convert Ensembl → Entrez
2. Many genes failed to map (lost ~20% of data)
3. clusterProfiler's `gseGO()` still failed

**Better Solution:** Use **MSigDB with fgsea**
- Works directly with Ensembl IDs (no conversion needed!)
- More comprehensive gene sets than GO alone
- Includes Hallmark, KEGG, Reactome, Immunologic signatures
```r
# This worked perfectly!
msigdb_hallmark <- msigdbr(species = "Mus musculus", category = "H")
fgsea(pathways = hallmark_list, stats = gene_list)
```

**Key Learning:** When one tool doesn't work, find a better tool rather than forcing it. Modern workflows (MSigDB + fgsea) often superior to older approaches (GO + clusterProfiler).

**Interpretation Challenges:**

**Question:** Why do both CD103+ vs CD49+ AND CD103+ vs DN show positive NES for the same pathways?

**Answer:** Because both comparisons are from CD103+'s perspective!
- Positive NES = enriched in CD103+ (compared to CD49+ OR DN)
- Since CD49+ ≈ DN, both comparisons should (and do) show similar patterns

**This confirmed:** The two-state model is correct.

### Phase 7: Visualization Strategy (Week 4-5)

**Goal:** Create publication-quality figures that tell the biological story

**Design Principles:**
1. **PCA first:** Show overall sample relationships
2. **Volcano plots:** Highlight key marker genes
3. **Heatmaps:** Show expression patterns across samples
4. **GSEA barplots:** Reveal pathway-level differences
5. **Comparison plot:** Demonstrate DN ≈ CD49+

**Color Choices:**
- Red (#E41A1C): CD103+ stem-like (warm = memory/longevity)
- Blue (#377EB8): CD49+ effector (cool = active/proliferative)
- Orange: DN (intermediate/undefined)

**Figure Iteration Process:**
- Version 1: Basic ggplot defaults
- Version 2: Adjusted sizes, added labels
- Version 3: Publication-quality themes, clear legends
- Final: Consistent style across all figures

**Most Challenging Figure:** Comparison barplot
- Problem: Column names embedded pathway names
- Solution: Extract with regex, clean for display
- Result: Clear side-by-side comparison showing DN ≈ CD49+

### Phase 8: Biological Interpretation (Week 5)

**Connecting Computational Results to Biology:**

**Finding 1:** CD103+ cells express Tcf7, Il7r, Slamf6 (stemness genes)

**Literature Support:**
> "Tcf7, Slamf6, Il2, Cxcr5, Sell, and Il7r are genes encoding proteins with a key role in memory T cell formation and stemness properties." (Paper)

**Conclusion:** Data validate the paper's findings

**Finding 2:** CD49+ cells express Pdcd1 (PD-1), Havcr2 (Tim-3), Tox (exhaustion markers)

**Literature Support:**
> "CD49a+ TRM subset has a more activated profile, with high expression levels of PD-1, Tim-3, Tox, and T-bet." (Paper)

**Conclusion:** Effector TRM show exhaustion/activation signatures

**Finding 3:** GSEA shows CD103+ enriched for interferon responses, CD49+ for cell cycle

**Biological Model:**
```
Stem-like (CD103+):           Effector (CD49+):
- Surveillance mode           - Active killing mode
- Long-term memory            - Short-term response  
- Low proliferation           - High proliferation
- TGF-β promoted              - IL-12 promoted
```

**This fits perfectly with:** Stem cell biology principles (quiescent vs. proliferative states)

### Mistakes Made & Lessons Learned

**Mistake 1:** Started coding before understanding biology
- **Consequence:** Initially focused on wrong comparisons
- **Fix:** Read paper first, then analyze
- **Lesson:** Domain knowledge > computational skills alone

**Mistake 2:** Assumed three groups = three cell types
- **Consequence:** Wasted time analyzing CD49+ vs DN (no differences!)
- **Fix:** Let PCA and DEG counts guide interpretation
- **Lesson:** Data can reveal simpler models than experimental design suggests

**Mistake 3:** Tried to force Entrez ID conversion
- **Consequence:** Lost 20% of genes, still got errors
- **Fix:** Switched to MSigDB with native Ensembl IDs
- **Lesson:** Modern tools often solve old problems better

**Mistake 4:** Created visualizations without clear narrative
- **Consequence:** Too many plots, unclear story
- **Fix:** Focused on figures that support main conclusions
- **Lesson:** Figures should tell a story, not just display data

### What I Would Do Differently

1. **Start with exploration:** Run PCA and clustering before any differential expression
2. **Check assumptions:** Verify gene ID formats, sample labels before quantification
3. **Automate more:** Write Snakemake workflow from the start
4. **Document earlier:** Keep a lab notebook of decisions and rationale
5. **Validate interactively:** Check a few known genes manually before running full GSEA

### Skills Developed Through Problem-Solving

1. **Debugging bioinformatics tools** (Salmon errors, R package conflicts)
2. **Resource management** (working within memory constraints)
3. **Data wrangling** (handling different ID formats, cleaning column names)
4. **Statistical interpretation** (understanding when differences are meaningful)
5. **Biological reasoning** (connecting genes → pathways → cell states)
6. **Scientific communication** (presenting results clearly)

### Most Valuable Learning

**The integration of:**
- **Biology** (what are TRM cells?)
- **Statistics** (what is significant?)
- **Computation** (how do I quantify this?)
- **Visualization** (how do I show this?)

**Is essential for meaningful bioinformatics analysis.**

Numbers without biological interpretation are just... numbers.

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
