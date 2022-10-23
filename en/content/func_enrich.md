# Function enrichment

## Over-representation analysis

**enrich** performs over-representation analysis for gene sets function exploration using GO, KEGG, Reactome based on clusterProfile (Wu et al., 2021) and ReactomePA (Yu and He, 2016). In addition, it could simplify GO terms enrichment results using simplifyEnrichment (Gu and Hübschmann, 2021).

Arguments:

* -i: input
* -od: output directory
* -db： database [GO|KEGG|Reactome]
* -ont:  GO ontology [ALL|BP|CC|MF]
* -pval : p-value
* -qval : q-value
* -org:  organism, 'hs' for human, 'mm' for mouse
* --simple: simplify the GO enrichment result
* -gene_id： gene ID type

code:

```bash
$ mkdir output/enrich -p
$ astk enrich -i result/fb_e11_based/sig01/fb_e11_13_SE.sig.dpsi \
    -ont BP -qval 0.1 -orgdb mm  -fmt png \
    -od output/enrich/fb_e11_13_SE --simple

$ tree output/enrich/fb_e11_13_SE
img/enrich/fb_e11_13_SE_doc_sc
├── GO.BP.qval0.1_pval0.1.csv
├── GO.BP.qval0.1_pval0.1.png
└── simgo
    ├── GO_BP_simple.csv
    └── GO_BP_simple.png

1 directory, 4 files

```

In the output, *.csv files store the results as text, and *png files display the content as images.

<img src='static/img/GO.BP.qval0.1_pval0.1.png' alt="GO.BP.qval0.1_pval0.1.png"></img>

<img src='static/img/GO_BP_simple.png' alt="GO_BP_simple.png"></img>

## ORA enrichment comparison

**enrichCompare** is used for comparing functional profiles among different experiments. **ecmp**  is the short alias.

Arguments:

* -i: input files
* -od: output directory
* -db: database [GO|KEGG|Reactome]
* -ont:  GO ontology [ALL|BP|CC|MF]
* -pval: p-value
* -qval q-value
* -org organism, 'hs' for human, 'mm' for mouse
* -gene_id： gene ID type

code

```bash
$ astk ecmp -i result/fb_e11_based/lenc/*/fb_e11_12_SE.sig.dpsi \
     -ont BP -org mm  -fmt png \
     -od output/enrich/fb_e11_12_SE_lc
```

<img src='static/img/GO.cmp.BP.qval0.1_pval0.1.png' alt="nease.png"></img>

## NEASE

**nease** functionally characterizes AS events by integrating pathways with structural annotations of proteinprotein interactions(Louadi et al., 2021). Currently, it only supports Human.

Arguments:

* -i: dpsi file
* -od : output directory
* -pval : p-value
* -org: only Human
* -db: database, [PharmGKB|HumanCyc|Wikipathways|Reactome|KEGG|SMPDB|Signalink|NetPath|EHMN|INOH|BioCarta|PID]

code:

```bash
mkdir output/nease
astk nease -i result/ni_adj_ct/sig01/0_3_SE.sig.dpsi \
    -pval 0.1 -db Reactome KEGG  -od output/nease/ni_0_3_SE

```

<img src='static/img/nease.png' alt="nease.png"></img>

## NEASE comparison

**neaseCompare** is used to compare NEASE enrichment result among different experiments. **necmp**  is the short alias.

Arguments:

* -i: dpsi files
* -od: output directory
* -qval: q-value
* -org: only Human
* -db: database, [PharmGKB|HumanCyc|Wikipathways|Reactome|KEGG|SMPDB|Signalink|NetPath|EHMN|INOH|BioCarta|PID]
* -xl: x labels
* -n: show the top n terms
* -fw: figure width
* -fh: figure height

```bash
astk necmp -i ni_adj_ct/sig01/{0_3,3_6,6_12}_SE.sig.dpsi \
    -od output/necmp -xl 0_3 3_6 6_12 -n 15 -qval 0.1 -fw 4 -fh 6
```

<img src='static/img/necmp.png' alt="necmp.png"></img>

<h2>References</h2>
<p>
Wu,T. et al. (2021) clusterProfiler 4.0: A universal enrichment tool for interpreting omics data. The Innovation, 2.
</p>
<p>
Gu,Z. and Hübschmann,D. (2021) simplifyEnrichment: an R/Bioconductor package for Clustering and Visualizing Functional Enrichment Results.
</p>
<p>
Louadi,Z. et al. (2021) Functional enrichment of alternative splicing events with NEASE reveals insights into tissue identity and diseases. Genome Biology, 22, 327.
</p>
<p>
Yu,G. and He,Q.-Y. (2016) ReactomePA: an R/Bioconductor package for reactome pathway analysis and visualization. Mol. BioSyst., 12, 477–479.
</p>
