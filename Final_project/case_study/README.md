# IBD / Th17 Pathway Case Study

## Overview

This case study examines the impact of GWAS discoveries on patenting activity for **Inflammatory Bowel Disease (IBD)** through the lens of the **Th17 cell differentiation pathway**. It focuses on spillover effects from the IL23R gene (Entrez ID: 149233) and its neighbors in the KEGG Th17 pathway.

## Study Design

### Disease Focus
- **Disease**: Inflammatory Bowel Disease (IBD)
- **MeSH ID**: D015212
- **Patent-gene-disease entries**: 17,749
- **Unique genes**: 3,995
- **Time period**: 2000-2020

### Pathway Focus
- **Pathway**: KEGG Th17 cell differentiation pathway
- **Seed gene**: IL23R (Entrez ID: 149233)
- **Spillover approach**: BFS (Breadth-First Search) up to 3 hops from IL23R
- **Final gene set**: 25 genes (including IL23R)

## Key Files

### Data Files
1. **`KEGG_data.csv`** (19.9 MB)
   - Full KEGG pathway data
   - Columns: `pathway_name`, `from_gene_entrez`, `to_gene_entrez`

2. **`ibd_patent_sample.parquet`** (239 KB)
   - Filtered patent data for IBD
   - Patents from 2000-2020, probability cutoff 0.99
   - Source: Full patent dataset with years and GWAS annotations

3. **`ibd_panel.parquet`** (95 KB)
   - Balanced panel: all IBD gene-disease pairs × years (2000-2020)
   - Aggregated to `num_patents` per gene-disease-year

4. **`th17_il23r_spillovers.csv`** (40 KB) - **LEGACY**
   - **25 Th17 pathway genes** filtered from IBD panel
   - Genes within 3 hops of IL23R in Th17 pathway
   - **Missing spillover metadata** (no gwas_level, gwas_year_chosen, etc.)
   - **Important**: Includes 5 direct GWAS targets (see methodology note below)

5. **`th17_panel_with_spillovers.parquet`** / `.csv` - **NEW (RECOMMENDED)**
   - Same 25 Th17 genes, same 525 rows as legacy file
   - **Includes full spillover metadata** from full pipeline:
     - `gwas_level` (0=direct GWAS, 1/2/3=spillover hop distance)
     - `gwas_year_chosen` (earliest GWAS/spillover year)
     - `gwas_1hop`, `gwas_2hop`, `gwas_3hop` (spillover years by distance)
     - `gwas_source_gene` (which gene provided the spillover)
     - `extensive_flag` (binary patent indicator)
   - ✅ Verified identical patent counts to legacy file
   - **Distribution**: 5 direct GWAS genes (level 0), 20 spillover genes (level 1)
   - **Created**: October 15, 2025 from full pipeline data

### Analysis Notebooks
1. **`creating_ibd_sample.ipynb`**
   - Filters full patent dataset for IBD (MeSH D015212)
   - Applies 0.99 probability cutoff
   - Creates balanced panel

2. **`creating_th17_spillovers.ipynb`**
   - Computes 1-3 hop neighbors of IL23R in Th17 pathway
   - Uses undirected BFS traversal
   - Filters IBD panel to Th17 genes

3. **`graphing_th17.ipynb`**
   - Visualization and analysis

### Comparison Files (Generated)
4. **`ibd_spillovers_from_full_pipeline.csv`**
   - IBD spillovers from the full pipeline for comparison
   - 5,100 genes (vs 25 in case study)

5. **`spillover_comparison_summary.csv`**
   - Comparison metrics between case study and full pipeline

## Methodology

### Data Filtering
All analyses use consistent filters:
- **Patent years**: 2000-2020
- **GWAS years**: ≤ 2020
- **Probability cutoff**: 0.99 (both `gene_prob` and `dis_prob`)

### Spillover Definition

The case study uses a **pathway-centric** approach:

1. **Start**: IL23R (gene_id: 149233) in KEGG Th17 pathway
2. **Traversal**: Undirected BFS to find genes within 1-3 hops
3. **Result**: 25 genes total
   - 1-hop: 4 genes (direct neighbors of IL23R)
   - 2-hop: 15 genes
   - 3-hop: 14 genes
   - Plus IL23R itself

### Important Methodological Note

**The case study includes 5 genes that are direct GWAS targets for IBD:**
- 3570 (IL6R)
- 4088 (SMAD3)
- 6097 (RORC)
- 6774 (STAT3)
- **149233 (IL23R)** ← the seed gene itself

This differs from the full pipeline, which **excludes direct GWAS targets** from spillovers by design. The case study's pathway-based approach includes these genes to show the complete Th17 pathway context, even though they are not technically "spillovers" from other GWAS discoveries.

### Relationship to Full Pipeline

The full pipeline (located in `../claude/`) processes **all** gene-disease combinations:
- **Full pipeline**: 5,100 spillover genes for IBD
  - Includes all KEGG pathways
  - Excludes direct GWAS targets
  - 1-hop: 1,481 genes | 2-hop: 2,766 genes | 3-hop: 853 genes

- **Case study**: 25 genes (subset)
  - Only Th17 pathway genes within 3 hops of IL23R
  - Includes direct GWAS targets
  - 20 genes overlap with full pipeline's spillovers
  - 5 genes are direct GWAS targets (excluded from full pipeline spillovers)

## Gene List

### The 25 Th17 Pathway Genes

**IL23R and direct neighbors (1-hop):**
- 149233 (IL23R) ← seed gene, direct GWAS target
- 4087 (SMAD2) ← direct GWAS target
- 4088 (SMAD3) ← direct GWAS target
- 4089 (SMAD4) ← direct GWAS target
- 6774 (STAT3) ← direct GWAS target

**2-hop neighbors:**
- 3091, 3561, 3570 (IL6R)*, 3572, 3594, 3605, 6095, 6097 (RORC)*, 7046, 7048, 50615, 50616, 50943, 59067, 112744
- *direct GWAS targets

**3-hop neighbors:**
- 861, 1432, 2475, 2625, 4772, 4773, 4775, 5600, 5603, 6300, 6776, 6777, 6778, 7040

## Data Consistency Check

✅ **IBD sample data is consistent** between case study and full pipeline (both created from same filters)

⚠️ **Spillover definitions differ** by design:
- Case study: Pathway-based (includes direct GWAS targets)
- Full pipeline: Pure spillovers (excludes direct GWAS targets)

## Source Data

- **Patent data**: `../patent/Full_Patent_Cleaned_with_Years_and_GWAS.parquet`
- **GWAS data**: `../gwas/GWAS_drop_level_1_2_and_6plus_CORRECTED_fuzzy90_dedup.tsv`
- **KEGG pathways**: `KEGG_data.csv` (in this directory)

## Full Pipeline Reference

For the complete spillover pipeline that processes all diseases:
- Pipeline scripts: `../claude/01_full_dataset_cleaning.py`, `02_spillover_creation.py`, `04_spillover_integration.py`
- Full spillover output: `../claude/full_spillovers_out/spillovers_pre_panel.parquet`
- Full panel with spillovers: `../claude/full_panel_with_spillovers.parquet`

## Usage Notes

When using this case study data:
1. Be aware that 5 of the 25 genes (including IL23R) are direct GWAS targets, not spillovers
2. Consider separating direct GWAS effects from spillover effects in econometric analysis
3. For pure spillover analysis, use only the 20 genes that overlap with the full pipeline
4. For pathway-context analysis, use all 25 genes

## Timeline

- **Case study created**: August 17, 2024
- **Full pipeline created**: September 26, 2024 (newer)
- **Comparison analysis**: October 15, 2025

## Questions?

For questions about methodology differences between case study and full pipeline, see:
- `spillover_comparison_summary.csv` - summary metrics
- `ibd_spillovers_from_full_pipeline.csv` - full pipeline IBD spillovers for comparison
