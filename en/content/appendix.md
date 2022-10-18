# Appendix

## Parameter/Option naming convention

For input parameter naming:

- -i: it means input. The name indicates that subsequent analysis only focuses on the AS event ID itself or the gene.
- -e: it means event. The name indicates that subsequent analysis will focuses on the each splicing site or element of AS event.

For output parameter naming:

- -o: it indicates that command output is a file or multiple file with different suffixes.
- -od: it indicates that command output is a directory.

## ASTK function dependencies

This is a table showing the **ASTK**'s non-Python dependencies.

| sub-commands| dependencies | command description |
| :---        |    :----     |   :----    |  
| barplot     | R(argparse, ggplot2, eoffice) | draw barplot       |
| heatmap     | R(argparse, ComplexHeatmap, eoffice) | draw heatmap plot      |
| upset     | R(argparse, UpSetR, eoffice) | draw UpSet plot       |
| volcano     | R(argparse, ggplot2, tidyverse, eoffice) | draw volcano plot       |
| pca     | R(argparse, tidyverse, eoffice) | draw heatmap  |
| enrich   |   R(argparse, clusterProfiler, ReactomePA, tidyverse)   |Over representation enrichment analysis |
| enrichCompare   | R(argparse, clusterProfiler, ReactomePA, tidyverse)  |enrichment comparision |
| motifEnrich   |   bedtools, meme-suite, R(memes)   |Motif Enrichment |
| motifFind   |   bedtools, meme-suite, R(memes)   |Motif Discovery and similarity comparision |
| spliceScore   |   bedtools | Compute 5/3 Splice site strength |
| gcc   |   bedtools | Compute GC content|
| sp2   |   R(argparse, metagene2, tidyverse, eoffice) | signal profile comparision|
| install   |   R | install dependencies packages|
