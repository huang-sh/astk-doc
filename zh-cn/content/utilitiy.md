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

## config

`config` sub-command could configure a specific R execution path for **ASTK** calling. If the R has installed **ASTK** R dependencies packages, this will avoid repeated installation and save a lot of time and resources. The **ASTK** sub-command dependencies can refer to [ASTK function dependencies](https://huang-sh.github.io/astk-doc/#/en/content/appendix?id=astk-function-dependencies)

Here is an example, you can configure the appropriate R path on your machine.

```bash
## search a R path
$ conda activate R41
$ which R
~/software/anaconda/envs/R41/bin/R

# return the astk environment
$ conda activate astk
# configure R path for astk
$ astk config -R ~/software/anaconda/envs/R41/bin/R
```

## merge

`merge`  is a sub-command for merge files into one.

Arguments:

- -i: input files
- -o: output path
- -axis: merge direction, 0 for row merge and 1 for column merge
- -rmdup: remove duplicate rows [index|all|content]
- -rmna:  remove NA data
