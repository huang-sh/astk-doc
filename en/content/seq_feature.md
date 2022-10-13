# 序列特征分析

## 序列特征提取

### 剪切位点强度

**ASTK**的剪切位点强度计算基于[MaxEntScan](http://hollywood.mit.edu/burgelab/maxent/download/fordownload/) (Yeo and Burge,2004) 的Perl脚本的重实现。

**spliceScore** 用于计算5'/3'剪切位点强度。**sss**是其别名。
参数设置如下：

* -e: AS事件文件地址
* -od：输出目录
* -fi：基因组fasta序列文件
* -app：事件是什么软件的输出 [auto|SUPPA2|rMATS]
* -p：进程数，默认为4

示例

```bash
$ mkdir sss
$ astk sss -e result/fb_e11_based/psi/fb_p0_SE_high.psi -od sss/fb_p0_SE_high -fi GRCm38.primary_assembly.genome.fa

$ tree sss/fb_p0_SE_high
├── A0_5SS
│   ├── A0_5SS.bed
│   └── A0_5SS.fa
├── A1_3SS
│   ├── A1_3SS.bed
│   └── A1_3SS.fa
├── A2_5SS
│   ├── A2_5SS.bed
│   └── A2_5SS.fa
├── A3_3SS
│   ├── A3_3SS.bed
│   └── A3_3SS.fa
├── splice_scores_box.png
└── splice_scores.csv

4 directories, 10 files
```

其中bed文件为bed 剪切位点区域的坐标，对于5'位点， exon部分3nt,intron 6nt, 共9nt的长度；对于3'位点， exon部分3nt,intron 20nt, 共23nt的长度。
其中fa文件为对应区域的fasta序列，对于负链的序列为反向互补序列。
splice_scores.csv 为剪切位点强度的csv文件
splice_scores_box.png剪切强度箱型图

<img src='static/img/splice_scores_box.png' alt="splice_scores_box.png"></img>

### GC 含量

**gcc** 用于计算各个剪切位点侧翼区间的GC含量值。
参数设置如下：

* -e: AS事件文件地址
* -od：输出目录
* -fi：基因组fasta序列文件
* -bs: bin size 或滑动窗口大小
* -ef: exon 侧翼长度
* -if：intron 侧翼长度
* -app：事件是什么软件的输出 [auto|SUPPA2|rMATS]
* -p：进程数，默认为4

示例：

```bash
$ mkdir gcc
$ astk gcc -e result/fb_e11_based/psi/fb_p0_SE_high.psi \
    -od gcc/fb_p0_AF_high -ef 150 -if 150 -bs 75 \
    -fi GRCm38.primary_assembly.genome.fa

$ tree gcc/fb_p0_AF_high
├── A1_5SS
│   ├── A1_5SS_dws.bed
│   ├── A1_5SS_dws.fa
│   ├── A1_5SS_dws_gcc.csv
│   ├── A1_5SS_ups.bed
│   ├── A1_5SS_ups.fa
│   └── A1_5SS_ups_gcc.csv
├── A2_3SS
│   ├── A2_3SS_dws.bed
│   ├── A2_3SS_dws.fa
│   ├── A2_3SS_dws_gcc.csv
│   ├── A2_3SS_ups.bed
│   ├── A2_3SS_ups.fa
│   └── A2_3SS_ups_gcc.csv
├── A3_5SS
│   ├── A3_5SS_dws.bed
│   ├── A3_5SS_dws.fa
│   ├── A3_5SS_dws_gcc.csv
│   ├── A3_5SS_ups.bed
│   ├── A3_5SS_ups.fa
│   └── A3_5SS_ups_gcc.csv
├── A4_3SS
│   ├── A4_3SS_dws.bed
│   ├── A4_3SS_dws.fa
│   ├── A4_3SS_dws_gcc.csv
│   ├── A4_3SS_ups.bed
│   ├── A4_3SS_ups.fa
│   └── A4_3SS_ups_gcc.csv
└── gcc.png

4 directories, 25 files  
```
<img src='static/img/gcc.png' alt="gcc.png"></img>

### exon/intron 长度

**elen** 用于计算相邻2个剪切位点之间的元件长度。
参数设置如下：

* -e: AS事件文件地址
* -od：输出目录
* -log: 将长度值进行log2转换

示例：

```basdh
$ mkdir len
$ astk elen -e result/fb_e11_based/psi/fb_e11_p0_AF_high.psi -od len/fb_p0_AF -log
$ ls len/fb_p0_A
element_len.csv  element_len.png
```

<img src='static/img/element_len.png' alt="element_len.png"></img>

## 序列特征比较

### 剪切位点强度比较

对两个条件下的剪切位点强度进行统计性比较。该功能可用**vcmp**实现。
其参数设置如下：

- -e: AS 事件剪切位点强度文件， 由**sss**生成
- -o：输出路径
- -test： 统计检验方法，可选[Mann-Whitney|t-test_ind|t-test_welch|Wilcoxon]，默认Mann-Whitney
- -gn： 组名 默认为 g1 g2
- -ft: 图形展示类型 [point|strip|box|boxen|violin|bar]，默认box
- -ff: 输出图片格式 [auto|png|pdf|tiff|jpeg]
- --xtitle：x轴标题
- --ytitle: y轴标题

示例

```bash
$ mkdir vcmp
$ astk vcmp -e sss/fb_e16_SE_*/splice_scores.csv \
    -gn high low -o vcmp/fb_e16_AF_sss.png  \
    --xtitle "splice_site" --ytitle "score" >  vcmp/fb_e16_AF_sss.txt
$ cat vcmp/fb_e16_AF_sss.txt   
p-value annotation legend:
      ns: p <= 1.00e+00
       *: 1.00e-02 < p <= 5.00e-02
      **: 1.00e-03 < p <= 1.00e-02
     ***: 1.00e-04 < p <= 1.00e-03
    ****: p <= 1.00e-04

A1_5SS_high vs. A1_5SS_low: Mann-Whitney-Wilcoxon test two-sided, P_val:1.243e-17 U_stat=6.122e+06
A0_pse_3SS_high vs. A0_pse_3SS_low: Mann-Whitney-Wilcoxon test two-sided, P_val:9.558e-01 U_stat=5.453e+06
A2_pse_3SS_high vs. A2_pse_3SS_low: Mann-Whitney-Wilcoxon test two-sided, P_val:3.981e-01 U_stat=5.515e+06
A3_5SS_high vs. A3_5SS_low: Mann-Whitney-Wilcoxon test two-sided, P_val:2.394e-26 U_stat=4.611e+06
A4_3SS_high vs. A4_3SS_low: Mann-Whitney-Wilcoxon test two-sided, P_val:4.759e-01 U_stat=5.392e+06
```

输出图片

<img src='static/img/fb_e16_AF_sss.png' alt="fb_e16_AF_sss.png"></img>

### exon/intron 长度比较

对两个条件下的exon/intron 长度进行统计性比较。该功能可用**vcmp**实现

- -e: AS 事件剪切位点强度文件， 由**elen**生成
- -o：输出路径
- -test： 统计检验方法，可选[Mann-Whitney|t-test_ind|t-test_welch|Wilcoxon]，默认Mann-Whitney
- -gn： 组名 默认为 g1 g2
- -ft: 图形展示类型 [point|strip|box|boxen|violin|bar]，默认box
- -ff: 输出图片格式 [auto|png|pdf|tiff|jpeg]
- -facet: 分面展示
- -log：数据进行log2转换
- --xtitle：x轴标题
- --ytitle: y轴标题
- --xlabel: x轴 label

示例：

```bash
astk vcmp -e Length/0hr_FT_se_*/element_len.csv \
        -gn high low -o vcmp/FT_se_lencmp.png -ft box \
        -log --xlabel ups_intron exon dws_intron \
        --xtitle "element" --ytitle "log2(length)" >  vcmp/FT_se_len.txt
```

<img src='static/img/FT_se_lencmp.png' alt="FT_se_lencmp.png"></img>

### GC 含量比较

对两个条件下的GC 含量进行统计性比较。该功能可用**vcmp**实现

- -e: AS 事件剪切位点强度文件， 由**gcc**生成，不支持滑动窗口设置
- -o：输出路径
- -test： 统计检验方法，可选[Mann-Whitney|t-test_ind|t-test_welch|Wilcoxon]，默认Mann-Whitney
- -gn： 组名 默认为 g1 g2
- -ft: 图形展示类型 [point|strip|box|boxen|violin|bar]，默认box
- -ff: 输出图片格式 [auto|png|pdf|tiff|jpeg]
- -facet: 分面展示
- -log：数据进行log2转换
- --xtitle：x轴标题
- --ytitle: y轴标题
- --xlabel: x轴 label

示例：

```bash
astk vcmp -e gcc/fb_p0_SE_*_b0/gcc.csv --facet \
        -gn high low -o vcmp/gcc/p0_se_gcccmp.png -ft box \
         --xtitle "splic_site" --ytitle "GCC" 
```

<img src='static/img/p0_se_gcccmp.png' alt="p0_se_gcccmp.png"></img>

<h2>参考文献</h2>
<p>
Yeo,G. and Burge,C.B. (2004) Maximum Entropy Modeling of Short Sequence Motifs with Applications to RNA Splicing Signals. 18.
</p>
