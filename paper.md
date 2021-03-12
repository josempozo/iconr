---
title: 'Analysis of Prehistoric Iconography with the R package iconr'
tags:
  - Iconography
  - Prehistory
  - Graph Theory
  - GIS
authors:
  - name: Thomas Huet^[Custom footnotes for e.g. denoting who the corresponding author is can be included like this.]
    orcid: 0000-0002-1112-6122
    affiliation: 1
  - name: Jose M Pozo
    orcid: 0000-0002-0759-3510
    affiliation: 2
  - name: Craig Alexander
    orcid: 0000-0001-7539-6415
    affiliation: 2
affiliations:
  - name: LabEx ARCHIMEDE, ANR-11-LABX-0032-01
    index: 1
  - name: Independent Researcher
    index: 2
date: 01 April 2021
bibliography: paper.bib
---

# Background

By definition, prehistorical societies are characterized by the absence of a writing system. During, the largest part of human history, and everywhere in the world, symbolic expressions belong mostly to illiterate societies which express themselves with rock-art paintings, pottery decorations, figurines and statuary, etc., and a lot of now disappeared carved woods, textile design, etc. These graphical expressions are the most significant remaining part of the humankind's symbolism. At the composition level, the presence of meaningful associations of signs and recurrent patterns (i.e., graphical syntax) clearly indicates the existence of social conventions in the way to display and to read these expressions. Well-established and shared methods to record and study these graphical contents would open the possibility of cross-cultural comparisons at a large scale and over the long-term.

# Statement of need

The inherent variability of ancient iconography has led to considerable problems in its study, drastically limiting the possibility to draw a synthesis of humankind's symbolism at a large scale and over the long-term:

 + Unexplicit spatial groupings of graphical units -- like graphical units grouped into *figures*, *figures* grouped into *patterns*, *patterns* grouped into *motives*, etc. -- introduce tedious number of groups and hinder their sistematic analysis.
 + Consistency, proximities and relationships between these groups are often implicit and not quantified. This is especially problematic for spatial proximities between graphical units, which are in general not quantified.
 + Studies develop their own descriptive vocabularies, singular relationships of grouping, and idosyncratic methods at site-dependent or period-dependent scales.
\end{itemize}

`iconr` is an R package designed to offer a greater normalization of quantitative indexes for iconography studies [@Alexander08; @HuetAlexander15; @Huet18a]. It is grounded in graph theory and spatial analysis to offer concepts and functions for modeling prehistoric iconographic compositions and preparing them for further analysis (clustering, typology tree, Harris diagram, etc.). The main principle of the `iconr` package is to consider any iconographic composition (here, 'decoration') as a geometric graph of graphical units. Geometric graphs are also known as *planar graph* or *spatialized graphs*. Graphical units are decorated surfaces (`POLYGONS`) modeled as nodes (`POINTS`). Graphical units representing attributes of a compound graphical unit are joined by directed edges to the corresponding compound's main node. Each pair of adjacent main nodes, sharing a border (*birel*: touches) of their Voronoi cells, are connected by an undirected edge (`LINES`). 
 
 
<center>

![GIS view. The Late Bronze Age stelae from Solana de Cabañas (Exteremadura, Spain). 1. Original photograph (credits: Museo Arqueológico Nacional, Madrid); 2. Archaeological drawing of engraved parts (credits: @DiazGuardamino10); 3. Digitalisation/Polygonization of engraved parts (i.e., graphical units) and calcul of their their centroids (red points); 4. Voronoi diagram of each graphical units (*seeds*) and dula graph of Voronoi diagram (i.e., Delaunay triangulation); 5. Identification of graphical units' types](https://raw.githubusercontent.com/zoometh/iconr/master/doc/img/solana_voronoi.png)

</center> 

The `iconr` package takes in charge the management of the geometric graphs (step 5 in the previous figure). Steps 1 to 4 do not need to be included in the package since efficient implementations already exist: graph elements can be drawn directly on the decorated support drawing or photograph, preferably inside a GIS to make easier the calculation of nodes and edges coordinates. The `iconr` package allows the user to i) read data structures of nodes and edges (.tsv, .csv, .shp) and images (.jpg, .png, .tif, .gif, etc.), ii) plot nodes and edges separately, or together (geometric graph), over the decoration picture, iii) compare different decorations depending on common nodes or common edges. The package stable version is on the CRAN [@iconr]; the latest development version is available from GitHub (https://github.com/zoometh/iconr); the package documentation is available at https://zoometh.github.io/iconr/.

# Example


```r
library(iconr)
dataDir <- system.file("extdata", package = "iconr")
imgs <- read.table(file.path(dataDir, "imgs.csv"), sep=";")
nodes <- read.table(file.path(dataDir, "nodes.csv"), sep=";")
edges <- read.table(file.path(dataDir, "edges.csv"), sep=";")
lgrph <- list_dec(imgs, nodes, edges)
df.same_edges <- same_elements(lgrph, "type", "edges")
df.same_nodes<- same_elements(lgrph, "type", "nodes")
dist.nodes <- dist(df.same_nodes, method = "euclidean")
dist.edges <- dist(df.same_edges, method = "euclidean")
hc.nds <- hclust(dist.nodes, method = "ward.D")
hc.eds <- hclust(dist.edges, method = "ward.D") 
par(mfrow=c(1, 2))
plot(hc.nds, main = "Common nodes", cex = .8)
plot(hc.eds, main = "Common edges", cex = .8)
```
<center>

![Results of the hierarchical clustering on the `iconr` decoration training dataset (five Late Bronze Age stelae) on common nodes (left) and common edges (right)](https://raw.githubusercontent.com/zoometh/iconr/master/doc/img/hc.png)

</center> 

# Acknowledgements

This  project was partly  supported  by  LabEx  ARCHIMEDE  from  “Investissement  d’Avenir”  program  ANR-11-LABX-0032-01.

# References