# Moitf analysis

In the module, **ASTK** implements RBP motif analysis using [meme-suite](https://meme-suite.org/) ((McLeay and Bailey, 2010; Bailey; Gupta et al., 2007;(Grant et al., 2011). **ASTK** collects RBP motifs from CIS-BP-RNA and ATtRACT database(Ray et al., 2013; Giudice et al., 2016) as ASTK built-in motif resources.

## Motif enrichment

We implement **motifEnrich** sub-command to perform motif enrichment analysis for checking RBP binding affinity difference within different condition and splicing sites flank sequences. **me** is the short alias.

Arguments:

* -te: treatment AS events
* -ce：control AS events
* -od：output dirctory
* -db： RBP motif database [ATtRACT|CISBP-RNA]
* -org: organism, 'hs' for human, 'mm' for mouse
* -fi: genome fasta

code:

```bash
$ mkdir output/motif
$ astk me -te result/fb_e11_based/psi/fb_p0_SE_high.psi \
    -ce result/fb_e11_based/psi/fb_p0_SE_low.psi \
    -od output/motif/fb_e11_p0_SE_me -org mm \
    -fi GRCm38.primary_assembly.genome.fa

$ tree output/motif/fb_e11_p0_SE_me
motif/fb_e11_p0_SE_me
├── A0_5SS
│   ├── A0_5SS.bed
│   ├── A0_5SS.fa
│   ├── ame.html
│   ├── ame.tsv
│   └── sequences.tsv
├── A1
│   ├── A1_ctrl.bed
│   └── A1_ctrl.fa
├── A1_3SS
│   ├── A1_3SS.bed
│   ├── A1_3SS.fa
│   ├── ame.html
│   ├── ame.tsv
│   └── sequences.tsv
├── A2
│   ├── A2_ctrl.bed
│   └── A2_ctrl.fa
├── A2_5SS
│   ├── A2_5SS.bed
│   ├── A2_5SS.fa
│   ├── ame.html
│   ├── ame.tsv
│   └── sequences.tsv
├── A3
│   ├── A3_ctrl.bed
│   └── A3_ctrl.fa
├── A3_3SS
│   ├── A3_3SS.bed
│   ├── A3_3SS.fa
│   ├── ame.html
│   ├── ame.tsv
│   └── sequences.tsv
├── A4
│   ├── A4_ctrl.bed
│   └── A4_ctrl.fa
└── heatmap.pdf
```

## Motif discovery

**motifFind** could perform motif discovery and compare motifs against a database of known motifs. **mf** is the short alias.

Arguments:

* -te: treatment AS events
* -ce：control AS events
* -od：output dirctory
* -db： RBP motif database [ATtRACT|CISBP-RNA]
* -org: organism, 'hs' for human, 'mm' for mouse
* -fi: genome fasta
* -pval: p-value

```bash
astk mf -te result/fb_e11_based/psi/fb_p0_SE_high.psi \
    -ce result/fb_e11_based/psi/fb_p0_SE_low.psi \
    -od output/motif/fb_e11_p0_SE_mf -org mm \
    -fi GRCm38.primary_assembly.genome.fa \
    -db CISBP-RNA

$ tree output/motif/fb_e11_p0_SE_mf
output/motif/fb_e11_p0_SE_mf
├── A0_5SS
│   ├── A0_5SS.bed
│   ├── A0_5SS_ctrl.bed
│   ├── A0_5SS_ctrl.fa
│   ├── A0_5SS.fa
│   ├── streme.html
│   ├── streme.txt
│   ├── streme.xml
│   ├── tomtom.html
│   ├── tomtom.tsv
│   └── tomtom.xml
├── A1_3SS
│   ├── A1_3SS.bed
│   ├── A1_3SS_ctrl.bed
│   ├── A1_3SS_ctrl.fa
│   ├── A1_3SS.fa
│   ├── streme.html
│   ├── streme.txt
│   ├── streme.xml
│   ├── tomtom.html
│   ├── tomtom.tsv
│   └── tomtom.xml
├── A2_5SS
│   ├── A2_5SS.bed
│   ├── A2_5SS_ctrl.bed
│   ├── A2_5SS_ctrl.fa
│   ├── A2_5SS.fa
│   ├── streme.html
│   ├── streme.txt
│   ├── streme.xml
│   ├── tomtom.html
│   ├── tomtom.tsv
│   └── tomtom.xml
└── A3_3SS
    ├── A3_3SS.bed
    ├── A3_3SS_ctrl.bed
    ├── A3_3SS_ctrl.fa
    ├── A3_3SS.fa
    ├── streme.html
    ├── streme.txt
    ├── streme.xml
    ├── tomtom.html
    ├── tomtom.tsv
    └── tomtom.xml
```

<h2>References</h2>
<p>
Giudice,G. et al. (2016) ATtRACT—a database of RNA-binding proteins and associated motifs. Database: The Journal of Biological Databases and Curation, 2016.
</p>
<p>
Ray,D. et al. (2013) A compendium of RNA-binding motifs for decoding gene regulation. Nature, 499, 172.
</p>
<p>
Bailey,T.L. STREME: accurate and versatile sequence motif discovery. 7.
</p>
<p>
Grant,C.E. et al. (2011) FIMO: scanning for occurrences of a given motif. Bioinformatics, 27, 1017–1018.
</p>
<p>
McLeay,R.C. and Bailey,T.L. (2010) Motif Enrichment Analysis: a unified framework and an evaluation on ChIP data. BMC Bioinformatics, 11, 165.
</p>
