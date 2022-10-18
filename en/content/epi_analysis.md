# Epigenetic signal analysis

**ASTK** implement the module for profiling splicing sites epigenetic signal of AS events.

## heatmap profile

**signalProfile** characterizes the epigenetic signal distribution of splice sites in heatmap manner.

Arguments:

* -e: AS event file
* -o: output path
* -bw: bigwig file
* -ssl: splicing site labels

code:

```bash
$ astk pf -i result/fb_e11_based/psi/fb_e11_16_AF_case.psi \
    -psi 0.8 -o result/fb_e11_based/psi/fb_16_AF_high.psi

$ astk pf -i result/fb_e11_based/psi/fb_e11_16_AF_case.psi \
    -psi -0.2 -o result/fb_e11_based/psi/fb_16_AF_low.psi

$ astk signalProfile -o output/fb_16_AF_high_ATAC.png \
    -e result/fb_e11_based/psi/fb_16_AF_high.psi \
    -bw ATAC.e16.5.fb.bigwig \
    -ssl A1 A2 A3 A4 A5 -fmt png

$ astk signalProfile -o output/fb_16_AF_low_ATAC.png \
    -e result/fb_e11_based/psi/fb_16_AF_low.psi \
    -bw ATAC.e16.5.fb.bigwig \
    -ssl A1 A2 A3 A4 A5 -fmt png
```

fb_16_AF_high_ATAC.png
<img src='static/img/AF_high.png' alt="AF_high.png"></img>

fb_16_AF_low_ATAC.png
<img src='static/img/AF_low.png' alt="AF_low.png"></img>

## Profile comparison

**sp2** sub-command is used for signal distribution comparison between the two condition.

Arguments:

* -mat: the output of **signalProfile**
* -o: output path
* -gn: group names
* --width: figure width
* --height: figure height

code

```bash

$ astk sp2 -mat output/fb_16_AF_*_ATAC.mat.gz \
    -o output/fb_e16.5_AF_atac.ecmp.pdf -gn high low --width 10 --height 8 
```

<img src='static/img/AF_high_vs_low.png' alt="AF_high_vs_low.png"></img>
