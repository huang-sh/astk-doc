# 可变剪切分析

## 差异可变剪切分析

### dsflow

**dsflow** 提供了可变剪切事件推断、可变剪切事件PSI计算、差异可变剪切分析和显著结果筛选。同时，该命令可以对多组对照组进行分析。

命令参数设置如下：

* -od: 输出目录
* -md: metadata json文件， 由命令**meta**生成
* -gtf: 基因组注释GTF文件
* -et: 可变剪切事件类型[ALL|SE|A5|A3|MX|RI|AF|AL]
* -m: 差异AS计算方法， [empirical|classical]
* -p：显著性AS事件p-val 阈值
* -adpsi: 显著性AS事件 dPSI 绝对阈值

示例：

```bash
$ mkdir result
$ astk dsflow -od result/fb_e11_based -md metadata/fb_e11_based.json \
    -gtf gencode.vM25.annotation.gtf -t ALL &

$ ls result/fb_e11_based
dpsi  psi  ref  sig01  tpm
```

输出目录包含四个文件夹：

* ref 用于存放可变剪切事件ioe参考文件
* tpm 用于存放转录本TPM 文件
* psi 用于存放可变剪切事件PSI文件
* dpsi 用于存放可变剪切事件dPSI文件
* sig01 用于存放统计显著性得差异可变剪切事件

### 单步运行

**ASTK** 中的可变剪切事件推断、PSI计算和差异可变剪切计算是基于SUPPA2算法原理发展而来。以下3个命令与SUPPA2中的命令同名，适合于单步分析计算。在大多数情况下，更推荐使用[dsflow](dsflow.html)

#### generateEvent

**generateEvent** 是基于基因组注释GTF文件来推断可变剪切事件。

**generateEvent** 参数设置如下:

* -gtf: 基因组注释GTF文件
* -et: 可变剪切事件类型 [ALL|SE|A5|A3|MX|RI|AF|AL]
* --split：根据AS事件的组成型exon是否为转录本第一个或者最后一个exon，[inner|FTE|LTE]
* -o: output path

示例：

```bash
$ astk generateEvent -gtf gencode.vM25.annotation.gtf  -et SE \
    -o result/fb_e11_based/ref/gencode.vM25

$ head gencode.vM25_SE_strict.ioe
seqname gene_id event_id        alternative_transcripts total_transcripts
chr1    ENSMUSG00000025900.13   ENSMUSG00000025900.13;SE:chr1:4293012-4311270:4311433-4351910:- ENSMUST00000208660.1    ENSMUST00000208660.1,ENSMUST00000208793.1
chr1    ENSMUSG00000025902.13   ENSMUSG00000025902.13;SE:chr1:4492668-4493100:4493466-4493772:- ENSMUST00000027035.9    ENSMUST00000027035.9,ENSMUST00000192650.5,ENSMUST00000191647.1
chr1    ENSMUSG00000025902.13   ENSMUSG00000025902.13;SE:chr1:4492668-4493100:4493490-4493772:- ENSMUST00000116652.7    ENSMUST00000116652.7,ENSMUST00000192650.5,ENSMUST00000191647.1
```

#### psiPerEvent

**psiPerEvent** 基于转录本TPM值来计算可变剪切事件的PSI值。

命令参数设置如下：

* -o: output path
* -qf: 转录本定量文件
* -ioe: 可变剪切事件参考ioe 文件，由**generateEvent**生成
* --tpmThreshold：转录本TPM值最低阈值

示例：

```bash
$ astk psiPerEvent -o fb_SE_e10.psi \
    -qf data/quant/fb_e10.5_rep*/quant.sf \
    -ioe gencode.vM25_SE_strict.ioe


$ head fb_SE_e10.psi
event_id        fb_e10.5_rep1   fb_e10.5_rep2
ENSMUSG00000025900.13;SE:chr1:4293012-4311270:4311433-4351910:- 1.0     1.0
ENSMUSG00000025902.13;SE:chr1:4492668-4493100:4493466-4493772:- 0.50685587094156        0.49103663421745114
ENSMUSG00000025902.13;SE:chr1:4492668-4493100:4493490-4493772:- 0.7405788514940302      0.697045848727448
ENSMUSG00000025902.13;SE:chr1:4493863-4495136:4495942-4496291:- 0.20508467461294735     0.16355428896798765

## 该命令也会从转转录本定量文件中提取转录本TPM数据
$ head fb_SE_e10.tpm -n 3
fb_e10.5_rep1   fb_e10.5_rep2
ENSMUST00000193812.1    0.0     0.0
ENSMUST00000082908.1    0.0     0.0
```

#### diffSplice

**diffSplice** 用于计算差异可变剪切事件。
命令参数设置如下：

* -psi: PSI文件，重复样本PSI值需要在同一个文件内
* -exp: 转录本TPM文件，重复样本TPM值需要在同一个文件内
* -ref: ioe 参考文件
* -o: 输出文件
* -m: 方法[empirical|classical]，default=empirical

示例：

```bash
$ astk diffSplice -psi fb_SE_e11.psi fb_SE_e16.psi\
    -exp fb_SE_e11.tpm b_SE_e16.tpm\
    -ref gencode.vM25_SE_strict.ioe \
    -o fb_SE_e10_e16.dpsi 
```

## 可变剪切事件处理

### dPSI 过滤

**sigFilter**用于根据dPSI和p-value 来筛选显著性可变剪切事件。目前支持过滤SUPPA2和rMATS得输出结果。**sf** 是**sigFilter**别名。

该命令参数设置如下：

* -i: 输入dpsi文件
* -o: 输出文件
* -adpsi: 绝对dPSI 阈值
* -p: p-value 阈值, 默认为0.05
* -dpsi: dPSI 阈值, 默认为0
* -sep: 如果设置，输出文件会根据 dPSI>0 和  dPSI<0 将过滤后得dpsi文件分为两个
* -app: 输入文件是什么软件的输出结果, 默认自动推断

### PSI 过滤

**psiFilter** 用于根据PSI值，进行可变剪切事件挑选。**pf** 是**psiFilter**别名。

该命令参数设置如下：

* -i: 输入psi文件
* -o: 输出文件
* -psi: PSI 阈值, 如果输入参数大于0，则挑选PSI大于该阈值的事件，如果小于0，则挑选PSI小于该阈值的事件
* -qt: 根据PSI 的百分比进行挑选

示例：

```bash
$ astk pf -i result/fb_e11_based/psi/fb_e11_p0_SE_case.psi \
    -psi 0.8 -o result/fb_e11_based/psi/fb_p0_SE_high.psi
$ astk pf -i result/fb_e11_based/psi/fb_e11_p0_SE_case.psi \
    -psi -0.2 -o result/fb_e11_based/psi/fb_p0_SE_low.psi
```

### 长度聚类

**lenCluster** 用于基于可变exon的长度进行分类可变剪切事件。**lc** 是**lenCluster**别名。

该命令参数如下:

* -i: 输入文件
* -lr: 长度聚类
* -od: 输出目录

示例：

```bash
$ astk lc -i result/fb_e11_based/sig01/*dpsi -lr 1 51 251 1001 \
    -od result/fb_e11_based/lenc

$ ls result/fb_e11_based/lenc
1-51  251-1001  51-251
```


## 可变剪切事件分析

### 事件分布

#### barplot

**barplot** 用于绘制不同条件、事件类型的可变剪切数目分布。

该命令参数设置如下：

* -i: 输入文件
* -o: 输出图片
* -dg: AS 事件将根据dPSI的正方划分为两组 (group +: dPSI > 0, group -: dPSI < 0)
* -xl: x 轴标签

示例：

```bash
astk barplot -i result/fb_e11_based/sig01/fb_e11_p0_*.sig.dpsi \
    -o img/fb_e11_p0_bar.png -dg -xl A3 A5 AF AL MX RI SE

```

![fb_e11_p0_bar.png](../../gitbook/images/fb_e11_p0_bar.png)

#### upset plot

**upset** 用于绘制可变剪切事件的在不同组的交集图。

参数设置如下：

* -i: dpsi文件
* -o: 输出文件
* -xl: x 轴标签
* -dg: AS 事件将根据dPSI的正方划分为两组 (group +: dPSI > 0, group -: dPSI < 0)

示例：

```bash
$ astk upset -i result/fb_e11_based/sig01/fb_e11_12_SE.sig.dpsi \
    result/fb_e11_based/sig01/fb_e11_14_SE.sig.dpsi \
    result/fb_e11_based/sig01/fb_e11_16_SE.sig.dpsi \
    -o img/fb_upset.png -xl e11_12 e11_14 e11_16 -dg

```

![fb_upset.png](../../gitbook/images/fb_upset_dg.png)

### PSI/dPSI 分析

#### PCA

**pca**根据PSI值来绘制PCA图.

参数设置如下：

* -i: psi文件
* -o: 输出图片路径
* -fmt: 图片格式
* --width: 图片长度
* --height：图片高度
* -gn: 分组名

示例：

```bash
$ astk pca -i result/fb_e11_based/psi/fb_e11_12_SE_c1.psi \
    result/fb_e11_based/psi/fb_e11_1[2-6]_SE_c2.psi \
    result/fb_e11_based/psi/fb_e11_p0_SE_c2.psi \
    -o img/fb_pca.png -fmt png --width 6 --height 4 \
    -gn fb_e11.5 fb_e12.5 fb_e13.5 fb_e14.5 fb_e15.5 fb_e16.5 fb_p0

```

![fb_pca.png](../../gitbook/images/fb_pca.png)

#### heatmap plot

**heatmap** 用于绘制PSI热图. **hm**是**heatmap**的别名.

参数设置如下：

* -i: psi文件
* -o: 输出图片
* -fmt: 图片格式

示例：

```bash
$ astk hm -i result/fb_e11_based/sig01/psi/fb_e11_12_SE_c1.sig.psi \
    result/fb_e11_based/sig01/psi/fb_e11_1*_SE_c2.sig.psi \
    -o img/fb_hm.png -fmt png

```

![fb_hm.png](../../gitbook/images/fb_hm.png)

#### volcano plot

**volcano** 用于绘制dPSI火山图。  **vol**是**volcano**的别名

参数设置如下:

* -i: dpsi文件
* -o: 输出文件

示例：

```bash
$ astk volcano -i result/fb_e11_based/dpsi/fb_e11_p0_SE.dpsi \
    -o img/fb_e11_p0_SE.vol.png 

```

![volcano.png](../../gitbook/images/fb_e11_p0_SE.vol.png)
