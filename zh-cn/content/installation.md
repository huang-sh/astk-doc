# 安装

**ASTK** 只适用于Linux 系统，如果需要在Window 系统使用，可以在Window上安装WSL 或者虚拟机。

## 常规安装

首先，利用conda 为**astk**创建一个虚拟环境，虽然这不是必须的，但是建议这样做。

```bash
## 创建一个新环境名为askt, 同时安装 Python 和 R
$ conda create -n astk -c conda-forge r-base=4.1 python=3.8 -y
## 激活环境 astk
$ conda activate astk
## 安装两个依赖软件
$ conda create -n astk -c bioconda bedtools meme -y
```

从PyPi安装**astk**（当前不要使用该安装方式!）

```bash
$ pip install astk
```

或者从github安装**astk**

```bash
## install the development version from github
$ pip install git+https://github.com/huang-sh/astk.git@dev

```

安装完**astk**后， 还需要安装一些依赖的R包， 建议配置一下特定的 R 执行路径，如果该R 已经安装了许多R 包，这会节省许多时间和资源。

这是一个例子， 你可以在你机器上配置合适的R路径。

```bash
$ conda activate R41
$ which R
~/software/anaconda/envs/R41/bin/R
```

**astk** 配置指定R.

```bash
astk config -R ~/software/anaconda/envs/R41/bin/R
```

> 这不是必须的，如果你的机器上没有合适的R，可以不用配置

之后，可以利用该行代码来安装依赖的R包，**astk** 会根据之前设置的R 来下载相应的依赖包，如果已存在就不会重复安装。所以对于一个已经安装了许多软件的R 而言，这会节省不少时间。如果之前未配置 R 路径，**astk** 会使用当前环境下的R。

```bash
$ astk install -r 
...
```

> 安装过程中，有些R包会安装失败， 可以尝试使用conda 来进行安装

## Docker 安装

如果你机器上可以使用Docker, 那直接使用Docker 来安装软件会节省不少时间。

```bash
$ docker pull huangshing/astk
 
```

### astk docker 使用

这里介绍一下如何使用docker 版的 **astk**

首先利用命令 `alias` 创建一个别名

```bash
$ alias astkdocker="docker run --rm -v /home/test/project:/project -e MY_USER=$(id -u) huangshing/astk"
```

其中将 `/home/test/project` 替换成你机器上的路径， 该路径下应该存放着，你需要分析的所有数据文件和参考文件等。

然后这样调用 **astk**

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
