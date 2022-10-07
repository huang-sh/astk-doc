# 可变剪切事件处理

## dPSI 过滤

**sigFilter**用于根据dPSI和p-value 来筛选显著性可变剪切事件。目前支持过滤SUPPA2和rMATS得输出结果。**sf** 是**sigFilter**别名。

该命令参数设置如下：

* -i: 输入dpsi文件
* -o: 输出文件
* -adpsi: 绝对dPSI 阈值
* -p: p-value 阈值, 默认为0.05
* -dpsi: dPSI 阈值, 默认为0
* -sep: 如果设置，输出文件会根据 dPSI>0 和  dPSI<0 将过滤后得dpsi文件分为两个
* -app: 输入文件是什么软件的输出结果, 默认自动推断

## PSI 过滤

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

## 长度聚类

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
