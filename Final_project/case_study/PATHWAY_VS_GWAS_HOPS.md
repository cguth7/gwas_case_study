# Understanding: Pathway Hops vs GWAS Spillover Hops

**Key Insight**: A gene can be far from IL23R in the Th17 pathway, but still close to other GWAS discoveries in the broader network.

---

## The Two Different "Hop" Concepts

### 1. **Pathway Hops** (Case Study BFS)
- **Measurement**: Distance from IL23R within the Th17 cell differentiation pathway
- **Method**: Breadth-first search on Th17 pathway edges only
- **Purpose**: Identify genes biologically related to IL23R through this specific pathway

### 2. **GWAS Spillover Hops** (Full Pipeline)
- **Measurement**: Distance from ANY GWAS discovery for IBD in the entire KEGG network
- **Method**: Network expansion from all GWAS genes across all pathways
- **Purpose**: Identify genes that could receive knowledge spillovers from GWAS discoveries

---

## Why They Differ

**Example**: Gene 7040
- **Pathway hop 3** from IL23R (far away in Th17 pathway)
- **GWAS spillover 1-hop** from gene 4088 (SMAD3)

**What happened?**
1. Gene 7040 is 3 steps from IL23R when you ONLY follow Th17 pathway edges
2. BUT gene 4088 (SMAD3) was discovered in GWAS for IBD
3. Gene 4088 is connected to gene 7040 in the KEGG network (possibly through a different pathway)
4. So gene 7040 gets a 1-hop spillover from 4088, even though it's 3 hops from IL23R

**Key**: The full KEGG network is much more densely connected than any single pathway!

---

## Complete Gene-by-Gene Breakdown

See `th17_pathway_vs_gwas_hops.csv` for the full table.

### Pathway Hop 0: IL23R itself (1 gene)
| Gene | Pathway Hop | GWAS Level | Source |
|------|-------------|------------|--------|
| 149233 (IL23R) | 0 | Direct GWAS | self |

**Interpretation**: IL23R discovered for IBD in 2007.

---

### Pathway Hop 1: Direct neighbors of IL23R (4 genes)

| Gene | Pathway Hop | GWAS Level | Source | Notes |
|------|-------------|------------|--------|-------|
| 4087 | 1 | 1-hop spillover | 149233 | Spillover from IL23R |
| 4088 (SMAD3) | 1 | Direct GWAS | self | Also a GWAS target! |
| 4089 | 1 | 1-hop spillover | 149233 | Spillover from IL23R |
| 6774 (STAT3) | 1 | Direct GWAS | self | Also a GWAS target! |

**Interpretation**:
- 2 genes (4088, 6774) were independently discovered in GWAS
- 2 genes (4087, 4089) get spillovers from IL23R

---

### Pathway Hop 2: Second-degree neighbors of IL23R (13 genes)

| Gene | Pathway Hop | GWAS Level | Source | Notes |
|------|-------------|------------|--------|-------|
| 3091 | 2 | 1-hop spillover | 6774 (STAT3) | Gets spillover from STAT3, not IL23R |
| 3561 | 2 | 1-hop spillover | 6774 | Gets spillover from STAT3 |
| 3570 (IL6R) | 2 | Direct GWAS | self | Direct GWAS target |
| 3572 | 2 | 1-hop spillover | 6774 | Gets spillover from STAT3 |
| 3594 | 2 | 1-hop spillover | 6774 | Gets spillover from STAT3 |
| 3605 | 2 | 1-hop spillover | 6774 | Gets spillover from STAT3 |
| 6097 (RORC) | 2 | Direct GWAS | self | Direct GWAS target |
| 7046 | 2 | 1-hop spillover | 4088 (SMAD3) | Gets spillover from SMAD3 |
| 50615 | 2 | 1-hop spillover | 6774 | Gets spillover from STAT3 |
| 50616 | 2 | 1-hop spillover | 6774 | Gets spillover from STAT3 |
| 50943 | 2 | 1-hop spillover | 4088 | Gets spillover from SMAD3 |
| 59067 | 2 | 1-hop spillover | 6774 | Gets spillover from STAT3 |
| 112744 | 2 | 1-hop spillover | 6774 | Gets spillover from STAT3 |

**Key Observations**:
- 2 genes (3570, 6097) are direct GWAS targets
- 11 genes get spillovers, but NOT from IL23R!
- 8 genes get spillovers from STAT3 (6774)
- 3 genes get spillovers from SMAD3 (4088)

**Why?** These genes are 2 hops from IL23R in the Th17 pathway, but they're connected to STAT3/SMAD3 in other pathways too. Since STAT3 and SMAD3 were discovered in GWAS, these genes get 1-hop spillovers from them.

---

### Pathway Hop 3: Third-degree neighbors of IL23R (7 genes)

| Gene | Pathway Hop | GWAS Level | Source GWAS Gene | Source Notes |
|------|-------------|------------|------------------|--------------|
| 861 | 3 | 1-hop spillover | 6097 (RORC) | Connected to RORC (hop 2 gene) |
| 1432 | 3 | 1-hop spillover | 2212 | Connected to OTHER GWAS gene (not in Th17) |
| 2475 | 3 | 1-hop spillover | 5579 | Connected to OTHER GWAS gene (not in Th17) |
| 4773 | 3 | 1-hop spillover | 3586 | Connected to OTHER GWAS gene (not in Th17) |
| 5600 | 3 | 1-hop spillover | 2212 | Connected to OTHER GWAS gene (not in Th17) |
| 6778 | 3 | 1-hop spillover | 3586 | Connected to OTHER GWAS gene (not in Th17) |
| 7040 | 3 | 1-hop spillover | 4088 (SMAD3) | Connected to SMAD3 (hop 1 gene) |

**Key Observations**:
- ALL 7 genes are 1-hop spillovers (no direct GWAS)
- 1 gene (861) gets spillover from RORC (a pathway hop 2 gene)
- 1 gene (7040) gets spillover from SMAD3 (a pathway hop 1 gene)
- 5 genes get spillovers from genes OUTSIDE the Th17 set (2212, 5579, 3586)

**Why?** These genes are 3 hops from IL23R in the Th17 pathway, but they're connected to:
- Other Th17 genes that were discovered in GWAS
- IBD GWAS genes that aren't even in the Th17 pathway

The KEGG network is highly interconnected!

---

## Summary Statistics

### By Pathway Hop from IL23R

| Pathway Hop | Total Genes | Direct GWAS | 1-hop Spillovers |
|-------------|-------------|-------------|------------------|
| 0 (IL23R) | 1 | 1 | 0 |
| 1 | 4 | 2 | 2 |
| 2 | 13 | 2 | 11 |
| 3 | 7 | 0 | 7 |
| **Total** | **25** | **5** | **20** |

### Spillover Sources for the 20 Spillover Genes

| Source Gene | In Th17 Set? | Pathway Hop | # Spillovers | Years |
|-------------|--------------|-------------|--------------|-------|
| 149233 (IL23R) | Yes | 0 | 2 | 2007 |
| 4088 (SMAD3) | Yes | 1 | 3 | varies |
| 6774 (STAT3) | Yes | 1 | 8 | varies |
| 6097 (RORC) | Yes | 2 | 1 | varies |
| 2212 | No | - | 2 | varies |
| 3586 | No | - | 2 | varies |
| 5579 | No | - | 1 | varies |

**Key Finding**:
- Only 14 of 20 spillovers come from genes within the Th17 set
- 6 spillovers come from IBD GWAS genes outside the Th17 pathway
- This shows the Th17 genes are embedded in a broader IBD network

---

## Implications for Analysis

### 1. **Treatment Heterogeneity**

The "spillover treatment" is not uniform:
- Some spillovers come from IL23R (the focal gene)
- Some come from other Th17 genes (within-pathway)
- Some come from non-Th17 IBD genes (cross-pathway)

### 2. **Timing Variation**

Different spillovers occurred at different times:
- IL23R spillovers: 2007
- STAT3 spillovers: varies by when STAT3 discovered
- Cross-pathway spillovers: depends on other GWAS discoveries

### 3. **Multiple Sources**

Some genes might be influenced by multiple GWAS discoveries:
- The pipeline keeps only the EARLIEST/CLOSEST
- But genes could be 1-hop from multiple GWAS sources

### 4. **Network Density**

The fact that all spillovers are 1-hop (not 2 or 3) shows:
- The KEGG network is densely connected
- Most genes are close to SOME GWAS discovery
- Hard to find genes truly "far" from GWAS in network terms

---

## Recommendations for Case Study

### When Analyzing Spillovers

**Option 1: IL23R-centric view (Original approach)**
- Focus on IL23R discovery (2007) as the key event
- Treat all other genes as "potentially affected"
- Simple pre/post comparison

**Option 2: Source-aware view (NEW)**
- Separate spillovers by source:
  - IL23R spillovers (2 genes)
  - STAT3 spillovers (8 genes)
  - SMAD3 spillovers (3 genes)
  - Other spillovers (7 genes)
- Control for different timing

**Option 3: Pathway distance view (Hybrid)**
- Use pathway hops as heterogeneity measure
- Check: Do hop-1 genes respond more than hop-3?
- Even though all are GWAS 1-hop, pathway proximity might matter

### Key Questions

1. **Do pathway-proximate genes (hop 1) respond more to IL23R discovery than distant genes (hop 3)?**
   - Even though all are GWAS 1-hop, IL23R's discovery might have larger effects on its direct pathway neighbors

2. **Do genes with GWAS sources inside vs outside Th17 differ?**
   - 14 spillovers from Th17 genes vs 6 from external genes
   - Within-pathway spillovers might be stronger

3. **Does multiplicity matter?**
   - Some genes are near multiple GWAS discoveries
   - Might get reinforcing spillovers

---

## Files

- **`th17_pathway_vs_gwas_hops.csv`** - Complete gene-level comparison table
- **`th17_panel_with_spillovers.parquet`** - Panel data with all spillover metadata
- **`KEGG_data.csv`** - Full pathway network for reference

---

## Visualization Ideas

1. **Network diagram**: Show Th17 pathway with GWAS genes highlighted
2. **Sankey diagram**: Flow from GWAS sources → spillover genes
3. **Matrix**: Pathway hop (rows) × GWAS source (columns) → count
4. **Timeline**: When each GWAS gene was discovered and who it spilled to

The key insight to communicate: **"Pathway distance ≠ Spillover distance"**
