# Th17 5-Seeds Visualizations

## Methodology

This folder contains network visualizations using **5 GWAS seeds within the Th17 pathway**:

- **Five GWAS seeds**: All IBD GWAS discoveries that are in the Th17 pathway
  - IL23R (2007), STAT3 (2008), SMAD3 (2010), RORC (2012), IL6R (2016)
- **Spillover method**: BFS from each seed independently, keeping minimum distance and earliest year
- **Distance calculation**: Minimum pathway hops to ANY of the 5 seeds
- **Gene set**: 25 genes in Th17 pathway with IBD patent data (2000-2020)

## Distance Distribution

- **Distance-0** (5 genes): IL23R, STAT3, SMAD3, RORC, IL6R - the GWAS seeds themselves
- **Distance-1** (17 genes): Most genes are now just 1 hop from some GWAS discovery
- **Distance-2** (3 genes): Only a few genes remain at distance-2
- **Distance-3** (0 genes): No genes are more than 2 hops from a discovery

## Key Difference from Single Seed

With multiple seeds, the apparent "spillover landscape" shrinks dramatically:
- Single seed: 1 seed, 4 at distance-1, 13 at distance-2, 7 at distance-3
- **Five seeds: 5 seeds, 17 at distance-1, 3 at distance-2, 0 at distance-3**

This reflects the reality that multiple pathway genes were independently discovered as IBD risk factors.

## Files

### Network Layouts (Spring Layout, Seeds in Center Circle)
Generated using older methodology - see parent directory for current files

### Hierarchical Layouts (Left-to-Right by Distance)
Generated using older methodology - see parent directory for current files

## Visual Features

### Node Styling
- **Colors**:
  - Red: Distance-0 (5 GWAS seeds)
  - Blue: Distance-1 (17 genes)
  - Green: Distance-2 (3 genes)
  - Orange: Distance-3 (not used - no genes)
- **Size**: Based on cumulative patents before/after each gene's specific GWAS year
- **Labels**: Gene names

### Important Note on Timing
Unlike the single-seed approach, each gene uses its **nearest seed's GWAS year** for before/after calculations:
- Genes near IL23R use 2007
- Genes near STAT3 use 2008
- Genes near SMAD3 use 2010
- etc.

This provides more accurate spillover timing attribution.

## Use Cases

### Best For:
1. **Econometric analysis** - proper timing attribution for multiple discoveries
2. **Robustness checks** - testing if results depend on distant spillovers
3. **Showing temporal complexity** - different genes had different discovery timings
4. **Understanding seed choice sensitivity** - how results change with methodology

### Advantages over Single Seed:
- More accurate timing: genes are assigned to their closest discovery
- Avoids misclassification: STAT3 is now distance-0, not distance-1
- Realistic treatment effects: accounts for multiple "interventions"

### Limitations:
- Still ignores IBD discoveries outside Th17 pathway
- More complex story (5 seeds vs 1)
- Harder to visualize seed positions (5 points vs 1)

## Key Insights

1. **Multiple discoveries matter**: Most genes (17/20 non-seeds) are distance-1
2. **Distance-2/3 nearly disappear**: Only 3 genes at distance-2, none at distance-3
3. **Proper attribution critical**: Same gene can be distance-1 vs distance-3 depending on methodology
4. **Temporal spread**: Discoveries span 2007-2016, affecting spillover timing

## Comparison to Other Methodologies

| Metric | Single Seed (IL23R) | 5 Th17 Seeds | All IBD Seeds |
|--------|---------------------|--------------|---------------|
| Seeds | 1 | 5 | 5 in Th17 + external |
| Distance-1 | 4 | 17 | 20 |
| Distance-2 | 13 | 3 | 0 |
| Distance-3 | 7 | 0 | 0 |
| Avg distance | 2.04 | 0.96 | 0.80 |

## Technical Details

- **Deduplication**: When multiple seeds reach same gene, keep minimum distance, then earliest year
- **Attribution**: Each gene assigned to one source (closest seed)
- **GWAS years**: Vary by seed (2007, 2008, 2010, 2012, 2016)
- **Data file**: `th17_5seeds_spillovers_with_hops.csv`

See `../GRAPH_COMPARISON_GUIDE.md` for detailed methodology comparison.
