# All IBD Seeds Visualizations

## Methodology

This folder contains network visualizations using **ALL IBD GWAS discoveries as seeds**:

- **All IBD GWAS seeds**: Every gene with an IBD GWAS association (hundreds), not just those in Th17
- **Spillover method**: BFS from all seeds through full KEGG network, filter to Th17 genes
- **Distance calculation**: Minimum pathway hops to ANY IBD GWAS discovery (inside or outside Th17)
- **Gene set**: 25 genes in Th17 pathway with IBD patent data (2000-2020)
- **External seeds shown**: 3 IBD GWAS hits outside Th17 that directly connect to Th17 genes

## Distance Distribution

- **Distance-0** (5 genes): IL23R, STAT3, SMAD3, RORC, IL6R - IBD GWAS seeds in Th17
- **Distance-1** (20 genes): ALL remaining Th17 genes are 1 hop from some IBD discovery
- **Distance-2** (0 genes): No genes are distance-2 or higher
- **Distance-3** (0 genes): No genes are distance-3 or higher

## External Seed Connections

Three IBD GWAS discoveries **outside the Th17 pathway** provide spillovers into Th17:
- **Gene 2212** (possibly JAK2 or similar)
- **Gene 3586** (possibly IL10 or similar)
- **Gene 5579** (possibly NKX2-3 or similar)

These are shown as **gray squares** with **dashed lines** connecting to Th17 genes.

## Key Difference from Other Approaches

This is the **most realistic** representation:
- Single seed: 1 seed, long distances (up to 3 hops)
- 5 Th17 seeds: 5 seeds, medium distances (up to 2 hops)
- **All IBD seeds: 5 in Th17 + 3 external, all genes distance-0 or distance-1**

This shows that nearly every Th17 gene is adjacent to SOME IBD discovery somewhere in KEGG.

## Files

### With External Seeds Visible
These versions show the 3 external IBD GWAS seeds as gray squares:

- `*_with_external_*.png` - Shows complete picture including external influences

### Network Layouts
- Spring layout with 5 Th17 seeds in center circle
- External seeds positioned on periphery (left side)
- Dashed lines from external seeds to Th17 genes

### Hierarchical Layouts
- Left-to-right flow: External seeds → Th17 seeds → Distance-1 spillovers
- Vertical separators between distance levels
- Clear labels showing source of spillovers

## Visual Features

### Node Styling
- **Th17 GWAS seeds** (5): Red circles
- **Distance-1 spillovers** (20): Blue circles
- **External IBD seeds** (3): Gray squares (smaller, fixed size)
- **Labels**: Gene names for Th17; gene ID + "(ext)" for external

### Edge Styling
- **Within Th17**: Solid gray lines (KEGG pathway connections)
- **From external**: Dashed lines (cross-pathway spillovers)

### Color Scheme
- Same as other methodologies for consistency
- Gray specifically chosen to indicate "outside the pathway"

## Use Cases

### Best For:
1. **Main paper figures** - shows complete, realistic spillover environment
2. **Demonstrating cross-pathway effects** - external discoveries matter
3. **Matching full pipeline** - uses same methodology as main analysis
4. **Understanding disease-centric view** - all IBD discoveries are relevant

### Advantages:
- **Most accurate**: Reflects true spillover timing and sources
- **Complete picture**: Shows both internal and external influences
- **Matches pipeline**: Same methodology as `th17_panel_with_spillovers.csv`
- **Demonstrates key point**: Pathway boundaries don't limit spillovers

### Why This Matters:
This methodology reveals that biological discoveries don't respect pathway boundaries. Genes in the Th17 pathway can receive spillovers from IBD discoveries in:
- Immune signaling pathways
- Cytokine networks
- JAK-STAT pathways
- NFkB pathways
- etc.

This is biologically realistic: IL23R is connected to many immune genes, not just Th17 components.

## Key Insights

1. **Spillover saturation**: Nearly all genes (20/20 non-seeds) are distance-1
2. **External influences**: 3 discoveries outside Th17 directly affect Th17 genes
3. **No distant spillovers**: With comprehensive seed set, no gene is >1 hop away
4. **Cross-pathway connections**: Dashed lines show how discoveries in one pathway affect another

## Comparison to Other Methodologies

| Metric | IL23R Only | 5 Th17 Seeds | All IBD Seeds |
|--------|------------|--------------|---------------|
| Seeds in Th17 | 1 | 5 | 5 |
| External seeds | 0 | 0 | **3** |
| Distance-1 genes | 4 | 17 | **20** |
| Distance-2 genes | 13 | 3 | **0** |
| Distance-3 genes | 7 | 0 | **0** |
| Avg distance to seed | 2.04 | 0.96 | **0.80** |

## Interpretation

The progression from single-seed → 5 seeds → all seeds shows:

1. **Choice of seeds matters**: Distance distributions change dramatically
2. **Realism increases**: More seeds = more accurate representation
3. **Spillovers compress**: With more seeds, apparent distances shrink
4. **Cross-pathway effects exist**: External discoveries provide spillovers

For **econometric analysis**, this methodology is most appropriate because:
- Proper timing: each gene assigned to nearest discovery
- Complete treatment: accounts for all relevant GWAS hits
- Realistic counterfactual: what if NO IBD discoveries existed?

## Technical Details

- **Seed selection**: All IBD GWAS hits from full GWAS database
- **Network**: Full KEGG, all pathways (not just Th17)
- **Filtering**: Results filtered to show only Th17 genes
- **External seed identification**: Sources of distance-1 genes that are not in Th17
- **Data file**: `th17_panel_with_spillovers.csv` (from full pipeline)

## Relationship to Full Pipeline

These visualizations show a **subset** of the full pipeline results:
- Full pipeline: All diseases, all genes, all pathways
- This visualization: One disease (IBD), one pathway (Th17), showing external connections

The methodology is identical; this just zooms in on IBD + Th17.

See `../GRAPH_COMPARISON_GUIDE.md` for detailed methodology comparison.
