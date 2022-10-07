# dsflow

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
  