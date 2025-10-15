# IL23R Single Seed Visualizations

## Methodology

This folder contains network visualizations using the **simplest spillover approach**:

- **Single GWAS seed**: IL23R (gene 149233, discovered 2007)
- **Spillover method**: Breadth-first search (BFS) from IL23R through KEGG Th17 pathway
- **Distance calculation**: Number of pathway hops from IL23R
- **Gene set**: 25 genes in Th17 pathway with IBD patent data (2000-2020)

## Distance Distribution

- **Distance-0** (1 gene): IL23R - the GWAS seed itself
- **Distance-1** (4 genes): SMAD2, SMAD3, SMAD4, STAT3 - direct neighbors of IL23R
- **Distance-2** (13 genes): HIF1A, IL2RG, IL6R, IL6ST, IL12RB1, IL17A, RORC, TGFBR1, IL21R, IL22, FOXP3, IL21, IL17F
- **Distance-3** (7 genes): RUNX1, MAPK14, MTOR, NFATC2, STAT6, TGFB1, MAPK11

## Files

### Network Layouts (Spring Layout, IL23R Centered)
- `network_after_gwas_white.png` - Patents after 2007 (white background)
- `network_before_gwas_white.png` - Patents before 2007 (white background)
- `network_after_gwas_black.png` - Patents after 2007 (black background)
- `network_before_gwas_black.png` - Patents before 2007 (black background)

### Hierarchical Layouts (Left-to-Right by Distance)
- `hierarchy_after_gwas_white.png` - Patents after 2007 (white background)
- `hierarchy_before_gwas_white.png` - Patents before 2007 (white background)
- `hierarchy_after_gwas_black.png` - Patents after 2007 (black background)
- `hierarchy_before_gwas_black.png` - Patents before 2007 (black background)

## Visual Features

### Node Styling
- **Base size**: 2000 (very large for readability)
- **Size scaling**: +250 per cumulative patent
- **Colors**:
  - Red: Distance-0 (IL23R)
  - Blue: Distance-1
  - Green: Distance-2
  - Orange: Distance-3
- **Labels**: Gene names (not IDs)
- **Border**: 5px white (white theme) or black (black theme)

### Text Styling
- **Node labels**: 15-16pt bold
- **Title**: 24pt bold
- **Legend**: 16pt
- **Distance headers**: 18pt (hierarchical only)

### Themes
- **White theme**: Black text on white background - better for printing/publication
- **Black theme**: White text on black background - better for presentations/slides

## Use Cases

### Best For:
1. **Explaining the spillover concept** - simplest story with one seed
2. **Showing pathway structure** - full distance spectrum (0-3)
3. **Pedagogical purposes** - easiest to understand
4. **Demonstrating BFS algorithm** - clear progression through network

### Limitations:
- Ignores other IBD GWAS discoveries in Th17 pathway (STAT3, SMAD3, RORC, IL6R)
- Misattributes timing for genes close to other discoveries
- Overstates distances (e.g., STAT3 is distance-1 from IL23R but is itself a GWAS seed)

## Key Insights

1. **Spillovers reach far**: Even distance-3 genes show patent activity
2. **Distance matters**: Distance-2 has the most patents overall (13 genes × activity)
3. **Before/After contrast**: Clear increase in patenting after 2007 IL23R discovery
4. **Network structure**: Hierarchical layout shows clear progression; network layout shows connectivity

## Comparison to Other Methodologies

- **vs. 5 Th17 seeds**: This approach has more distance-2/3 genes (16 vs 3)
- **vs. All IBD seeds**: This approach has distance-2/3 genes (16 vs 0)

See `../GRAPH_COMPARISON_GUIDE.md` for detailed methodology comparison.

## Technical Details

- **Node positioning**:
  - Network: Spring layout with IL23R fixed at center
  - Hierarchical: Fixed x-position by distance, spread vertically
- **Edge rendering**: Undirected KEGG pathway relationships
- **Figure size**: 24×24 (network), 30×22 (hierarchical)
- **Resolution**: 300 DPI
- **Format**: PNG with appropriate background color
