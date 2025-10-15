# Th17 Pathway Visualizations - Version 2

This directory contains high-quality network visualizations for the IBD/Th17 pathway case study, organized by spillover methodology.

## Directory Structure

### Organized by Methodology

```
graphs_v2/
├── il23r_single_seed/          # Single seed (IL23R only)
│   ├── README.md
│   ├── network_*.png           # Spring layouts
│   └── hierarchy_*.png         # Left-to-right hierarchical
│
├── th17_5seeds/                # Five Th17 GWAS seeds
│   ├── README.md
│   └── (older versions)
│
├── all_ibd_seeds/              # All IBD seeds (with external)
│   ├── README.md
│   └── (older versions with external nodes)
│
└── GRAPH_COMPARISON_GUIDE.md   # Detailed methodology comparison
```

## Three Spillover Methodologies

### 1. IL23R Single Seed (`il23r_single_seed/`)
**Simplest approach** - spillovers from one landmark discovery

- 1 seed: IL23R (2007)
- Distance-1: 4 genes
- Distance-2: 13 genes
- Distance-3: 7 genes

**Best for**: Explaining concepts, pedagogical use, showing pathway structure

### 2. Five Th17 Seeds (`th17_5seeds/`)
**Multi-seed approach** - accounts for multiple discoveries in pathway

- 5 seeds: IL23R, STAT3, SMAD3, RORC, IL6R (2007-2016)
- Distance-1: 17 genes
- Distance-2: 3 genes
- Distance-3: 0 genes

**Best for**: Econometric robustness, proper timing attribution, showing temporal complexity

### 3. All IBD Seeds (`all_ibd_seeds/`)
**Complete picture** - includes external discoveries

- 5 seeds in Th17 + 3 external IBD GWAS seeds
- Distance-1: 20 genes (all remaining genes)
- Distance-2/3: 0 genes
- Shows cross-pathway spillover effects

**Best for**: Main paper figures, matching full pipeline, demonstrating realism

## File Naming Convention

### Current (il23r_single_seed/)
- `network_[timeframe]_gwas_[theme].png` - Spring layout
- `hierarchy_[timeframe]_gwas_[theme].png` - Left-to-right layout

Where:
- `timeframe`: `before` (2000-2007) or `after` (2008-2020)
- `theme`: `white` (black text) or `black` (white text)

### Legacy (root directory)
Older versions with various naming schemes - see subdirectories for current versions

## Visual Features

All visualizations share:
- **Node colors**: Red (distance-0), Blue (distance-1), Green (distance-2), Orange (distance-3)
- **Node size**: Proportional to cumulative patents (2000 base + 250 per patent)
- **Labels**: Gene names (e.g., IL23R, STAT3) instead of IDs
- **Fonts**: Large, readable (15-24pt)
- **Resolution**: 300 DPI for publication quality

### Layout Types

**Network layouts**:
- Spring layout algorithm
- Seeds positioned at center
- Shows full connectivity
- Better for understanding topology

**Hierarchical layouts**:
- Left-to-right by distance level
- Vertical separation lines
- Distance labels at top
- Better for presentations

### Themes

**White theme** (recommended for papers):
- White background
- Black text and node borders
- Better for printing

**Black theme** (recommended for slides):
- Black background
- White text and node borders
- More dramatic, better for presentations

## Key Visualizations by Use Case

### For Main Paper Figure
→ `all_ibd_seeds/` versions with external nodes visible

### For Methods Explanation
→ `il23r_single_seed/hierarchy_after_gwas_white.png`

### For Robustness Checks
→ `th17_5seeds/` versions

### For Presentations
→ Any `*_black.png` versions (black background)

## Distance vs. Hops Terminology

All visualizations use **"distance"** to refer to pathway hops from GWAS seeds. This is clearer than "hop level" used in earlier versions.

## Data Sources

- **Patent data**: `th17_il23r_spillovers_with_hops.csv` (single seed)
- **Patent data**: `th17_5seeds_spillovers_with_hops.csv` (5 seeds)
- **Patent data**: `th17_panel_with_spillovers.csv` (all IBD seeds)
- **KEGG pathways**: `KEGG_data.csv`
- **GWAS data**: `../gwas/GWAS_drop_level_1_2_and_6plus_CORRECTED_fuzzy90_dedup.tsv`

## Generation Scripts

All visualizations generated with Python using:
- `networkx` for layout algorithms
- `matplotlib` for rendering
- Custom styling for publication quality

See individual README files in subdirectories for specific parameters.

## Comparison Guide

For detailed methodology comparison and guidance on which visualizations to use for different purposes, see:

**`GRAPH_COMPARISON_GUIDE.md`**

This guide includes:
- Side-by-side methodology comparison
- Distance distribution tables
- Use case recommendations
- Methodological implications
- Technical details

## Updates

- **v2** (Current): Large nodes (2000 base), gene names, organized subdirectories
- **v1** (Legacy): Smaller nodes, gene IDs, less organized

## Questions?

See README files in each subdirectory for methodology-specific details, or consult `GRAPH_COMPARISON_GUIDE.md` for comparative analysis.
