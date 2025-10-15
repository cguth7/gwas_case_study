# Th17 Pathway Visualization Guide

## Overview

This document explains the different network visualizations created for the IBD/Th17 pathway case study. Each set of graphs illustrates a different spillover methodology, helping readers understand how the choice of GWAS seeds affects the spillover landscape.

## Key Terminology

- **Distance**: The number of pathway hops from a GWAS seed to a target gene (replaces "hop level")
- **GWAS Seed**: A gene with a direct GWAS association for IBD
- **Spillover**: A gene that receives a spillover from a GWAS seed through pathway connections
- **Th17 Pathway**: KEGG's Th17 cell differentiation pathway (25 genes with IBD patent data)

## Three Spillover Methodologies

### 1. Single Seed: IL23R Only
**Files**: `th17_network_*` and `th17_hierarchy_*`

**Methodology**:
- Start with IL23R (gene 149233) as the only GWAS seed
- Perform BFS (breadth-first search) to find neighbors within Th17 pathway
- All spillovers trace back to IL23R

**Results**:
- 1 seed (distance-0): IL23R
- 4 genes at distance-1
- 13 genes at distance-2
- 7 genes at distance-3

**Why This Matters**:
This represents the "purist" pathway-based approach. It shows how innovation diffuses from a single landmark discovery (IL23R, discovered 2007) through the biological pathway network. The visualization clearly demonstrates:
- How far removed genes are from the original discovery
- That distance-2 and distance-3 genes still receive meaningful spillovers
- The pure pathway structure without confounding from multiple discovery timings

**Use Case**: Best for understanding the pathway structure and showing how a single discovery can catalyze innovation across an entire biological system.

---

### 2. Five Seeds: All Th17 GWAS Hits
**Files**: `th17_5seeds_network_*` and `th17_5seeds_hierarchy_*`

**Methodology**:
- Start with all 5 IBD GWAS hits that are in the Th17 pathway
  - IL23R (2007), STAT3 (2008), SMAD3 (2010), RORC (2012), IL6R (2016)
- Perform BFS from each seed independently
- For genes reached by multiple paths, keep the minimum distance and earliest GWAS year

**Results**:
- 5 seeds (distance-0)
- 17 genes at distance-1
- 3 genes at distance-2
- 0 genes at distance-3

**Why This Matters**:
This methodology accounts for the fact that multiple genes in the pathway were independently discovered as IBD risk factors. Key insights:
- Most genes (17) are now only 1 hop away from a GWAS discovery
- The "apparent" spillover landscape shrinks dramatically
- Distance-2 and distance-3 genes nearly disappear because they have shorter paths to other seeds
- More accurately reflects the temporal complexity: different genes had different discovery years

**Use Case**: Best for econometric analysis where you want to properly attribute spillover timing. Each gene's "treatment" is based on its closest GWAS neighbor, avoiding misclassification of genes that were close to other discoveries.

---

### 3. All IBD Seeds: Complete Spillover Picture
**Files**: `th17_all_ibd_seeds_with_external_network_*` and `th17_all_ibd_seeds_with_external_hierarchy_*`

**Methodology**:
- Start with ALL IBD GWAS hits (~hundreds of genes), not just those in Th17
- Perform BFS through the full KEGG network
- Filter results to only show Th17 pathway genes
- Visualize external IBD GWAS seeds that connect to Th17

**Results**:
- 5 seeds in Th17 (distance-0)
- 20 genes at distance-1 (includes spillovers from external IBD discoveries)
- 0 genes at distance-2 or distance-3
- 3 external IBD GWAS seeds shown (genes 2212, 3586, 5579)

**Why This Matters**:
This is the most realistic representation of the actual spillover environment. Key insights:
- Nearly all Th17 genes (20/20 non-seeds) are distance-1 from SOME IBD discovery
- External discoveries (outside Th17) can still influence Th17 genes through pathway connections
- The dashed lines from external seeds (gray squares) show these cross-pathway influences
- This matches the full pipeline methodology used in the main analysis

**Visual Elements**:
- **Gray squares**: IBD GWAS hits outside the Th17 pathway
- **Dashed lines**: Connections from external seeds into Th17
- **Red circles**: IBD GWAS hits within Th17 (distance-0)
- **Blue circles**: Distance-1 spillovers

**Use Case**: Best for showing the complete picture and explaining why the full pipeline includes spillovers from all KEGG pathways. Demonstrates that biological discoveries don't respect pathway boundaries.

---

## Visualization Types

### Network Layouts
- Nodes positioned using spring layout
- GWAS seeds positioned at/near center
- Shows the full connectivity structure
- Better for understanding overall network topology

### Hierarchical Layouts
- Left-to-right flow by distance level
- Vertical separation lines between distances
- Shows clear progression from seeds to spillovers
- Better for presentations and explaining the concept of "distance"

### Before vs. After GWAS
Each methodology includes two timeframes:
- **Before GWAS** (2000-2007 for IL23R; varies for multi-seed): Cumulative patents before the relevant GWAS discovery
- **After GWAS**: Cumulative patents after the relevant GWAS discovery

Node size = cumulative patents × 50 (minimum 100 for visibility)

---

## Comparing the Methodologies

| Aspect | IL23R Only | 5 Th17 Seeds | All IBD Seeds |
|--------|------------|--------------|---------------|
| Seeds | 1 | 5 | 5 in Th17 + 3 external |
| Distance-1 genes | 4 | 17 | 20 |
| Distance-2 genes | 13 | 3 | 0 |
| Distance-3 genes | 7 | 0 | 0 |
| Avg. distance to nearest seed | 2.04 | 0.96 | 0.80 |
| External influences | No | No | Yes (3 seeds) |
| Pathway scope | Th17 only | Th17 only | All KEGG |

---

## Methodological Implications

### For Regression Analysis
- **IL23R Only**: Simplest, but misattributes timing for genes near other discoveries
- **5 Th17 Seeds**: Better timing attribution, but ignores external discoveries
- **All IBD Seeds**: Most accurate spillover classification (used in main analysis)

### For Storytelling
- **IL23R Only**: Best for explaining the basic concept of pathway spillovers
- **5 Th17 Seeds**: Best for showing temporal complexity of multiple discoveries
- **All IBD Seeds**: Best for showing the complete innovation ecosystem

### Trade-offs
Each methodology represents a different lens:
1. **Biological purity** (IL23R only) vs. **Empirical realism** (All IBD seeds)
2. **Simplicity** (single seed) vs. **Accuracy** (multiple seeds with proper timing)
3. **Pathway-centric** (Th17 only) vs. **Disease-centric** (all IBD discoveries)

---

## Recommended Usage

**For main paper figures**: Use "All IBD Seeds" visualizations
- Shows the complete picture
- Matches the pipeline methodology
- Demonstrates cross-pathway spillovers

**For methods explanation**: Use "IL23R Only" visualizations
- Clearest illustration of BFS concept
- Shows full distance spectrum (0-3)
- Simplest story

**For robustness checks**: Use "5 Th17 Seeds" visualizations
- Tests sensitivity to seed selection
- Shows how distance distributions change
- Validates that results aren't driven by distant spillovers

**For presentations**: Use hierarchical layouts
- Clearer left-to-right flow
- Easier to explain distance concept
- Better labeled with distance counts

---

## Technical Notes

- All visualizations use the same 25 Th17 genes (those with IBD patent data)
- Node colors consistent across all graphs: Red (distance-0), Blue (distance-1), Green (distance-2), Orange (distance-3), Gray (external)
- Patent data spans 2000-2020
- All edges represent undirected KEGG pathway relationships
- Distance calculation uses standard BFS with earliest-year priority for tie-breaking

---

## File Naming Convention

```
th17_[methodology]_[layout]_[timeframe]_gwas.png

methodology:
  - (none) = IL23R only
  - 5seeds = 5 Th17 GWAS seeds
  - all_ibd_seeds_with_external = All IBD seeds

layout:
  - network = Spring layout
  - hierarchy = Left-to-right hierarchical

timeframe:
  - before = Cumulative patents before GWAS
  - after = Cumulative patents after GWAS
```

## Summary

These visualizations collectively tell the story of how GWAS discoveries catalyze follow-on innovation through biological pathway networks. The progression from single-seed to multi-seed to all-seeds methodology demonstrates:

1. **Pathway spillovers are real**: Even distance-3 genes show patent activity
2. **Multiple discoveries matter**: Most genes are close to some GWAS hit
3. **Cross-pathway influences exist**: External discoveries affect pathway genes
4. **Proper timing attribution is critical**: Distance to nearest seed varies dramatically by methodology

Understanding these differences is essential for interpreting the econometric results and appreciating the complexity of knowledge spillovers in biomedicine.
