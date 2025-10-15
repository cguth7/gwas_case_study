# Th17 Pathway Case Study: Analysis Plan

**Goal**: Examine spillover effects from IL23R GWAS discovery (2007) on Th17 pathway genes for IBD patenting

**Data**: `th17_panel_with_spillovers.parquet` - 25 genes × 21 years (2000-2020) with full spillover metadata

---

## Context from Previous Analysis

### What Was Done (August 2024)
- Created 5 simple bar charts showing patent trends
- Applied "pre-GWAS filter" (exclude post-GWAS years for each gene)
- Key event line at 2007 (IL23R discovery)
- Separate plots: IL23R only, non-IL23R, all genes (mean and total)

### Key Findings to Explore
- 25 genes in Th17 pathway (IL23R + neighbors)
- 5 are direct GWAS targets (level 0): IL23R, IL6R, SMAD3, RORC, STAT3
- 20 are spillovers (level 1 in full pipeline)
- IL23R discovered for IBD in 2007
- Other GWAS discoveries at different times (see `gwas_year` column)

---

## Proposed Analysis Plan (Start Simple!)

### Phase 1: Descriptive Statistics (DO THIS FIRST)

**Goal**: Understand the data before plotting

1. **Gene-level summary table**
   - For each of 25 genes: total patents, GWAS year, spillover level, source gene
   - Identify which are direct GWAS (level 0) vs spillovers (level 1)
   - Show spillover timing (gwas_year_chosen)

2. **Time series summary**
   - Total patents per year (all genes)
   - Count of genes with patents per year
   - Patents by spillover level (direct vs spillover)

3. **Key events timeline**
   - When was each direct GWAS gene discovered?
   - When did spillover assignments occur?
   - How many years between events?

**Output**: Simple markdown table or CSV with summary stats

---

### Phase 2: Simple Visualizations

**Goal**: Replicate and improve previous plots with new data

#### Plot 1: IL23R Time Series (Updated)
- Bar chart: IL23R patents by year
- Vertical line at 2007 (discovery)
- Now can add: `gwas_year_chosen` annotation
- Check if we see post-discovery increase

#### Plot 2: Direct GWAS vs Spillovers (NEW)
- Two lines/bars:
  - Direct GWAS genes (level 0): 5 genes
  - Spillover genes (level 1): 20 genes
- Show mean patents per year for each group
- Vertical lines at key GWAS discovery years

#### Plot 3: Spillover Timing (NEW)
- For the 20 spillover genes, when did they get spillover assignments?
- Histogram of `gwas_year_chosen` for spillovers
- Shows clustering of spillover "treatments"

#### Plot 4: Pre vs Post (Simple Diff-in-Diff style)
- Split at 2007 (IL23R discovery - the main event)
- Compare: pre-2007 vs post-2007 means
- For: direct GWAS genes vs spillover genes
- Simple bar chart showing the difference

**Output**: 4 clean plots saved to `graphs_v2/` folder

---

### Phase 3: Gene-Specific Analysis

**Goal**: Look at individual gene trajectories

#### Plot 5: Individual Gene Time Series
- Small multiples (5x5 grid)
- One subplot per gene
- Color by spillover level (red = direct, blue = spillover)
- Show their GWAS/spillover year as vertical line

#### Plot 6: Top Movers (NEW)
- Identify genes with biggest change pre/post 2007
- Show top 5 gainers and top 5 losers
- Bar chart of percent change

**Output**: 2 more detailed plots

---

### Phase 4: Simple Regression (If Time)

**Goal**: Quantify the spillover effect

#### Analysis 1: Event Study Around IL23R
- Dependent var: patents per gene-year
- Treatment: spillover indicator × post-2007
- Control: gene fixed effects, year fixed effects
- Just show coefficients plot (β by year relative to 2007)

#### Analysis 2: Compare Direct vs Spillover
- Do direct GWAS genes respond differently than spillovers?
- Simple t-test or regression with interaction
- Table of results

**Output**: 1-2 simple regression tables/plots

---

## Data Preparation Notes

### Use the NEW file: `th17_panel_with_spillovers.parquet`
- Has full spillover metadata
- 525 rows (25 genes × 21 years)
- Key columns:
  - `gene_id`, `patent_year`, `num_patents`
  - `gwas_year` (direct GWAS year, null for non-direct)
  - `gwas_level` (0 = direct, 1 = spillover, null = neither)
  - `gwas_year_chosen` (earliest GWAS/spillover year)
  - `gwas_source_gene` (which gene caused the spillover)

### Treatment Definitions

**Option A: IL23R-centric (Original approach)**
- Treatment event: 2007 (IL23R discovery)
- Treated: All 24 other genes (mix of direct + spillovers)
- Control: Pre-2007 vs Post-2007

**Option B: Spillover-specific (NEW - Better)**
- Direct GWAS genes (5): Use their own discovery year
- Spillover genes (20): Use their spillover year (`gwas_year_chosen`)
- Can do staggered diff-in-diff

**Recommendation**: Start with Option A (simpler), then try Option B

---

## Code Structure

### Suggested notebook: `th17_analysis_v2.ipynb`

```python
# Section 1: Load and explore
- Load th17_panel_with_spillovers.parquet
- Basic df.info(), df.describe()
- Create summary tables

# Section 2: Simple plots
- Replicate old plots with new data
- Add spillover-aware plots

# Section 3: Gene-level analysis
- Individual trajectories
- Identify top movers

# Section 4: Statistical analysis (optional)
- Event study
- Simple regression
```

---

## Specific Questions to Answer

1. **Did IL23R patents increase after 2007 discovery?**
   - Simple pre/post comparison

2. **Did spillover genes increase after IL23R discovery?**
   - Compare 20 spillover genes pre vs post 2007

3. **Do direct GWAS genes respond differently than spillovers?**
   - 5 direct vs 20 spillovers

4. **Is the effect immediate or delayed?**
   - Look at year-by-year pattern

5. **Which genes benefited most from spillovers?**
   - Rank by change in patents

6. **Does hop distance matter?**
   - Note: In our data, all spillovers are 1-hop (level 1)
   - But original BFS had 1/2/3-hop structure - can reference that

---

## Key Methodological Decisions

### 1. What years to include?
- **Old approach**: Excluded post-GWAS years for each gene
- **New approach**: Keep all years, control for treatment timing
- **Recommendation**: Try both, compare

### 2. What's the control group?
- **Option A**: Pre-treatment years (within-gene comparison)
- **Option B**: Non-Th17 IBD genes (between-gene comparison)
- **Recommendation**: Start with Option A (simpler)

### 3. How to aggregate?
- Mean vs total patents
- Per-gene vs pooled
- **Recommendation**: Show both, emphasize mean (intensive margin)

---

## Deliverables

### Minimum (Phase 1-2):
- Summary statistics table
- 4 clean visualizations
- Brief markdown writeup of findings

### Ideal (Phase 1-4):
- All of above +
- Individual gene analysis
- Simple event study plot
- Short interpretation section

---

## Notes for Next Claude

- Start with Phase 1 (descriptive stats) - don't jump to plotting!
- Use the NEW file with spillover metadata
- Keep plots SIMPLE (no fancy styling needed)
- Focus on telling the story: Did spillovers matter?
- Save all outputs to `graphs_v2/` folder
- Create a summary markdown file with findings

The goal is to show whether knowledge spillovers from IL23R discovery affected patenting in connected genes. Keep it clean and interpretable!
