# 差异可变剪切分析

## dsflow

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

## 单步运行

**ASTK** 中的可变剪切事件推断、PSI计算和差异可变剪切计算是基于SUPPA2 (Trincado,J.L. et al., 2018) 算法原理发展而来。以下3个命令与SUPPA2中的命令同名，适合于单步分析计算。在大多数情况下，更推荐使用[dsflow](dsflow.html)

### generateEvent

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

### psiPerEvent

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

### diffSplice

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

<h2>参考文献</h2>
<p>
Trincado,J.L. et al. (2018) SUPPA2: fast, accurate, and uncertainty-aware differential splicing analysis across multiple conditions. Genome Biol., 19, 40.
</p>
