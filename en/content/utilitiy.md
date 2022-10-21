# Useful utilities

## install

`install` is a sub-command for install R packages.

Arguments:

- -cran: install R packageS from CRAN
- -bioc: install R packageS from Bioconductor
- -m: using tsinghua mirror
- -r: install all R dependencies packages for **ASTK**

install all R dependencies packages for **ASTK**:

```bash
astk install -r
```

However, you could install a few R packages for individual **ASTK** sub-command. The **ASTK** sub-command dependencies can refer to [ASTK function dependencies](https://huang-sh.github.io/astk-doc/#/en/content/appendix?id=astk-function-dependencies)

For example, you could install `UpSetR`, `argparse` and `eoffice` for `astk upset`:

```bash
astk install -cran ggplot2 argparse UpSetR
```

Sometimes, it may fail to install some packages, you can manually install relevant R packages.

For example, it may raise below error when you install `eoffice`:

```text
ERROR: dependencies ‘devEMF’, ‘magick’ are not available for package ‘eoffice’
```

You could install `magick` via `conda` or other ways.

```bash
# the magick package is named r-magick in conda-forge channel
$ conda install -c conda-forge r-magick
$ astk install -cran eoffice
```
