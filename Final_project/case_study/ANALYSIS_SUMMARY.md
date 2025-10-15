# Case Study vs Full Pipeline: Analysis Summary

**Date**: October 15, 2025
**Analyst**: Claude Code

## Executive Summary

✅ **Data consistency confirmed**: IBD patent samples are identical between case study and full pipeline
⚠️ **Methodological difference identified**: Case study includes 5 direct GWAS targets that full pipeline excludes

## Key Findings

### 1. IBD Sample Data: IDENTICAL ✅

| Metric | Old Sample | New Sample (from full pipeline) |
|--------|------------|--------------------------------|
| Total rows | 17,749 | 17,749 |
| Unique genes | 3,995 | 3,995 |
| Unique gene-disease pairs | 3,995 | 3,995 |
| Difference | 0 | 0 |

**Conclusion**: No need to re-pull IBD sample; data sources are consistent.

---

### 2. Spillover Comparison: METHODOLOGICAL DIFFERENCE

#### Scale Difference
- **Full pipeline IBD spillovers**: 5,100 genes
  - 1-hop: 1,481 genes
  - 2-hop: 2,766 genes
  - 3-hop: 853 genes
  - Source GWAS genes: 121 (1-hop), 96 (2-hop), 65 (3-hop)

- **Case study Th17 spillovers**: 25 genes
  - Focused subset: Th17 pathway only
  - Within 3 hops of IL23R (gene_id: 149233)

#### Overlap Analysis
- **Genes in BOTH**: 20 genes (all 1-hop spillovers in full pipeline)
- **Genes ONLY in case study**: 5 genes
- **Genes ONLY in full pipeline**: 5,080 genes (other pathways)

---

### 3. The 5 Missing Genes: Root Cause Analysis

**These genes are in the case study but NOT in the full pipeline spillovers:**

| Gene ID | Gene Symbol | Status |
|---------|-------------|---------|
| 149233 | IL23R | ✅ Direct GWAS target for IBD |
| 3570 | IL6R | ✅ Direct GWAS target for IBD |
| 4088 | SMAD3 | ✅ Direct GWAS target for IBD |
| 6097 | RORC | ✅ Direct GWAS target for IBD |
| 6774 | STAT3 | ✅ Direct GWAS target for IBD |

**Why the difference?**

**Full Pipeline Logic** ([02_spillover_creation.py](../claude/02_spillover_creation.py)):
```python
# Lines 181-201: Remove direct GWAS targets from spillovers
direct_pairs = set(zip(gwas_direct["gene_id"], gwas_direct["disease_id"]))
hops = hops.loc[~pd.Series(direct_mask)].copy()  # Exclude direct pairs
```

**Case Study Logic** ([creating_th17_spillovers.ipynb](creating_th17_spillovers.ipynb)):
```python
# Filter IBD panel to genes in Th17 pathway (no GWAS exclusion)
allowed_genes = set([IL23R_GENE_ID] + level_1_genes + level_2_genes + level_3_genes)
ibd_df_filtered = ibd_df[ibd_df['gene_id'].isin(allowed_genes)]
```

**This is BY DESIGN, not a bug:**
- Full pipeline: Pure spillover definition (excludes direct targets)
- Case study: Pathway context (includes all Th17 genes regardless of GWAS status)

---

## Why This Matters for Analysis

### Econometric Implications

The 5 direct GWAS genes (including IL23R) will show:
- **Stronger patent response** (they're the direct GWAS discoveries)
- **Earlier timing** (they're discovered in GWAS, not spilled over)
- **Different mechanism** (direct incentive vs indirect spillover)

### Recommended Approaches

#### Option A: Keep as-is (Pathway Context)
```
Pros: Shows full Th17 pathway story, includes the "hero gene" IL23R
Cons: Mixing direct and spillover effects
Use case: Descriptive analysis, pathway visualization
```

#### Option B: Exclude direct GWAS targets (Pure Spillovers)
```
Pros: Clean spillover identification, matches full pipeline methodology
Cons: Loses IL23R and 4 key pathway genes
Use case: Causal spillover estimation
```

#### Option C: Separate Analysis (Recommended for papers)
```
Create two specifications:
  1. Direct GWAS effects (5 genes)
  2. Spillover effects (20 genes)

Report both coefficients to show the difference in magnitude
This provides the cleanest identification strategy
```

---

## Full Pipeline Context

The full pipeline has **5,100 spillover genes for IBD** because:
1. It uses the **entire KEGG network** (not just Th17 pathway)
2. It includes spillovers from **121+ source GWAS genes** for IBD
3. It computes spillovers across **all pathways**, not just one

The case study's 25 genes are a **targeted subset** for focused analysis of the Th17 mechanism.

---

## Verification Steps Completed

✅ Re-pulled IBD sample from full pipeline → Identical to original
✅ Compared spillover gene lists → Found 5-gene difference
✅ Checked GWAS data → Confirmed all 5 are direct targets
✅ Reviewed pipeline code → Confirmed exclusion is intentional
✅ Generated comparison files for further analysis

---

## Generated Files

1. **`ibd_patent_sample_NEW.parquet`** - Re-pulled IBD sample (identical to old)
2. **`ibd_spillovers_from_full_pipeline.csv`** - All 5,100 IBD spillovers from full pipeline
3. **`spillover_comparison_summary.csv`** - Summary statistics
4. **`README.md`** - Comprehensive documentation
5. **`ANALYSIS_SUMMARY.md`** - This file

---

## Next Steps / Recommendations

1. **For descriptive analysis**: Use current case study as-is (all 25 genes)

2. **For causal analysis**: Consider creating two specifications:
   - `th17_direct_gwas.csv` - 5 direct GWAS genes
   - `th17_spillovers_only.csv` - 20 spillover genes

3. **For robustness checks**: Compare results against full pipeline's 5,100 IBD spillovers

4. **Documentation**: When writing up results, clearly note that:
   - IL23R and 4 neighbors are direct GWAS targets
   - Remaining 20 genes are true spillovers
   - Results may differ in magnitude between these groups

---

## Questions for Discussion

1. **Should we create separate files for direct vs spillover genes?**
   - This would make econometric analysis cleaner
   - Easy to do with a simple filter

2. **Do we want to add timing information?**
   - e.g., when was each gene discovered in GWAS?
   - Could show "treatment timing" for event study designs

3. **Should the case study match full pipeline methodology exactly?**
   - Would require excluding the 5 direct GWAS genes
   - Loses the focal gene (IL23R) from analysis

---

## Technical Notes

- All comparisons done with `/usr/bin/python3` (user's environment)
- Parquet and CSV formats used for cross-compatibility
- Gene IDs are Entrez format (integer)
- Disease IDs are MeSH format (string like "D015212")
