# Moitf 分析

对可变剪切位点侧翼序列进行motif分析， **ASTK** 会对所有剪切位点进行Motif分析。依赖于[meme-suite](https://meme-suite.org/) 实现。**ASTK** 内置了CIS-BP-RNA 和 ATtRACT 两个RBP 数据库， 可直接使用这两个数据库。

## Motif富集分析

Motif富集分析可用于查看不同条件下RBP结合亲和力差异。**ASTK** 使用子命令**motifEnrich**来实现，**me**是别名。
参数设置如下：

* -te: 处理组可变剪切事件
* -ce：对照组可变剪切事件
* -od： 输出目录
* -db： RBP motif数据库[ATtRACT|CISBP-RNA]
* -org: 物种
* -fi: 基因组fasta序列

示例

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

## Motif发现

**ASTK** 使用子命令**motifFind**来实现motif的发现和与已知RBP motif进行比对，**mf**是别名。
参数设置如下：

* -te: 处理组可变剪切事件
* -ce：对照组可变剪切事件
* -od： 输出目录
* -db： RBP motif数据库[ATtRACT|CISBP-RNA]
* -org: 物种
* -fi: 基因组fasta序列
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
