# Installation

At the moment, **ASTK** only supports Linux and OSX but not Windows operating systems. However, you could use **ASTK** on Windows system with a virtual machine or WSL installed.

## general installation

First, you should create a virtual environment for **ASTK** using conda.Although it is not required, but recommended.

```bash
## create a new environment named astk , install Python and R
conda create -n astk -c conda-forge r-base=4.2 python=3.9 -y
# activate  astk environment
conda activate astk
## install meme-suite for motif analysis
conda install -c bioconda meme -y
```

You could install a development version from github:

```bash
## install the development version from github
pip install git+https://github.com/huang-sh/astk.git@dev
# or
pip install git+ssh://git@github.com/huang-sh/astk.git@dev

```

After installing **ASTK**, you could install all dependent R package using below command:

```bash
astk install -r 
```

**Note!!!**
>  It will take a lot of time to install all dependent R packages. If you can refer to [astk install](https://huang-sh.github.io/astk-doc/#/en/content/utilitiy) to use astk to quickly achieve the analysis you want.

## Docker installation

It will save a lot of time using Docker directly if your machine already has docker installed.

```bash
docker pull huangshing/astk
```

### astk docker usage

Here, it's necessary to introduce how to use the docker version of **ASTK** more conveniently.

Run the command below, but replace the `/home/test/project` with your own. Notably, the new path should contain all files that **ASTK** need.

```bash
alias astkdocker="docker run --rm -v /home/test/project:/project -e MY_USER=$(id -u) huangshing/astk"
```

Then it call **ASTK** like:

```bash
$ astkdocker astk meta -h
Usage: astk meta [OPTIONS]

  generate metadata template file

Options:
  -c1, --ctrl PATH              file path for condtion 1  [required]
  -c2, --case PATH              file path for condtion 2  [required]
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
