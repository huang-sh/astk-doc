# Installation

At the moment, **ASTK** only supports Linux and OSX but not Windows operating systems. However, you could use **ASTK** on Windows system with a virtual machine or WSL installed.

## general installation

First, you should create a virtual environment for **ASTK** using conda.Although it is not required, but recommended.

```bash
## create a new environment named astk , install Python and R
conda create -n astk -c conda-forge r-base=4.1 python=3.8 -y
# activate  astk environment
conda activate astk
## install two dependencies softwares
conda create -n astk -c bioconda bedtools meme -y
```

**ASTK** is available from the Python Package Index (PyPI), it could bed installed like this:

```bash
pip install astk
```

You could install a development version from github:

```bash
## install the development version from github
pip install git+https://github.com/huang-sh/astk.git@dev

```

After installing **ASTK**, you also need to install some dependent R packages. It is recommended to configure a specific R execution path. If the R has installed some R packages, this will avoid repeated installation, so it can save a lot of time and resources. .

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

> It is not necessary. And if you don't have a suitable R on your machine, you can leave it out.

After that, you can use follow command to install the dependent R package.

```bash
astk install -r 
...
```

> Some R packages will fail to install, you could try to install package with conda.

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
