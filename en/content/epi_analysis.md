# Epigenetic signal analysis

**ASTK** implement the module for profiling splicing sites epigenetic signal of AS events.

## Extract signal

**signalExtract** could extract bigwig signal of splice sites

Arguments:

* -e: AS event file
* -o: output path
* -bw: bigwig file
* -ssl: splicing site labels

code:

```bash
$ astk pf -i result/fb_e11_based/psi/fb_e11_16_AF_c2.psi \
    -minq 0.8 -o result/fb_e11_based/psi/fb_16_AF_high.psi

$ astk pf -i result/fb_e11_based/psi/fb_e11_16_AF_c2.psi \
    -maxq 0.2 -o result/fb_e11_based/psi/fb_16_AF_low.psi


$ astk signalExtract -e result/fb_e11_based/psi/fb_16_AF_high.psi \
    -bw ATAC.e16.5.fb.bigwig -bs 5 -o output/fb_16_AF_high_ATAC.b5.csv

$ astk signalExtract -e result/fb_e11_based/psi/fb_16_AF_low.psi \
    -bw ATAC.e16.5.fb.bigwig -bs 5 -o output/fb_16_AF_low_ATAC.b5.csv
```
## Extract signal

**signalHeatmap** is used to plot signal heatmap

Arguments:

* -s: AS event signal CSV file
* -o: output path


```bash
$ astk shm -s output/fb_16_AF_high_ATAC.b5.csv --label high \
        -o output/AF_high.png
$ astk shm -s output/fb_16_AF_low_ATAC.b5.csv --label low \
        -o output/AF_low.png

$ astk shm -s output/fb_ATAC_16_AF_{high,low}.b5.csv --label high low\
        -o output/AF_high_vs_low.png
```

fb_16_AF_high_ATAC.png
<img src='static/img/AF_high.png' alt="AF_high.png"></img>

fb_16_AF_low_ATAC.png
<img src='static/img/AF_low.png' alt="AF_low.png"></img>

AF_high_vs_low.png
<img src='static/img/AF_high_vs_low.png' alt="AF_high_vs_low.png"></img>
