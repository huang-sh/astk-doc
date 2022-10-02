# Installation

## General installation

Create a virtual environment for ASTK, although this is not necessary.

```bash
## create a new conda environment for astk and install python and R
$ conda create -n astk -c conda-forge -c bioconda r-base=4.1 python=3.8 -y
$ conda create -n astk -c bioconda bedtools meme -y
## activate conda environment
$ conda activate astk
```

install ASTK using pip

```bash
## install the development version from github
$ pip install git+https://github.com/huang-sh/astk.git@dev

```

After installed **astk**, you should install **astk**' dependent R packages.
Then you can set a existing R executable binary file path, it will save lots of time and resource to install R packages.

It is an example, you can get yours R PATH on your own machine.

```bash
$ conda activate R41
$ which R
~/software/anaconda/envs/R41/bin/R
```

Configure the R path.

```bash
astk config -R ~/software/anaconda/envs/R41/bin/R
```

> Note that this is not required if you don't have R.

And install R packages with:

```bash
$ astk install -r 
...
```

> Note, Some R packages may fail to install, you can refer to FAQs for some solutions.

## Docker installation

This way will save much time to install software.

```bash
$ docker pull huangshing/astk
 
```

### astk docker usage

We could create a shortcut for the docker command with alias command. It is convenient for us to run the docker version of ASTK multiple times.

```bash
$ alias astkdocker="docker run --rm -v /home/test/project:/project -e MY_USER=$(id -u) huangshing/astk"
```

Please replace `/home/test/project`  with your path. This directory should contain some reference files and all files you need to analyze.

```
$ ll -h | cut -d " " -f 5-

  446M Aug 12 21:08 ATAC.e16.5.fb.bigwig
    27 Aug  8 16:40 data
  1.4G Aug  8 22:03 gencode.v38.annotation.gtf
  847M Aug  8 17:35 gencode.vM25.annotation.gtf
  2.6G Aug  8 17:35 GRCm38.primary_assembly.genome.fa

```

And then we just run astk like:

```
$ astkdocker astk meta -h
Usage: astk meta [OPTIONS]

  generate metadata template file

Options:
  -p1, --control PATH           file path for condtion 1  [required]
  -p2, --treatment PATH         file path for condtion 2  [required]
  -gn, --groupName TEXT         group name
  -repN, --replicate INTEGER    replicate, number
  -o, --output PATH             metadata output path  [required]
  -repN1, --replicate1 INTEGER  replicate1, number
  -repN2, --replicate2 INTEGER  replicate2, number
  --condition TEXT              condition name
  -fn, --filename               file name
  --split TEXT                  name split symbol and index
  -h, --help                    Show this message and exit.

```
