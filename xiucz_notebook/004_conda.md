##

```python
## 设置channel，用于让conda寻找软件所在位置
conda config --add channels conda-forge
conda config --add channels defaults
conda config --add channels r
conda config --add channels bioconda
## 已经添加的channels: conda config --get channels
## 更新condaconda update conda
## conda默认安装软件的最新版本，如果想安装指定版本的某个软件，可以先搜索软件版本conda search 软件名
## 星号标记的表示是已经安装的版本。要安装其他版本，输入：conda install 软件名=版本号
## 再次查看已安装软件列表conda list
```
## 添加镜像源
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

conda config --set show_channel_urls yes

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
```

## 推荐新建工作环境下安装软件;
```python
conda create -n aligners bwa bowtie hisat star -y
# create -n aligner:新建一个名为alinger的工作环境
# bwa bowite hista star： 同时安装这三个软件
# -y yes：和Linux的选项一样
```
## 常用命令
```
source activate env_name

source deactivate env_name

conda env list #查看所有创建的env, 其中处于active状态的env前会有一个*

conda create --clone py35 --name py35-2 #精确的copy一个env，并取一个新名称

conda list --revisions #列举出当前环境的所有历史变动信息

conda list #列出当前激活环境中的所有安装的packages信息

conda list --explicit >bio-env.txt #将当前环境的所有安装包信息导入到文件中，可以用于后续创建相同的环境

conda env create --file bio-env.txt #创建新的环境并从文件中获得package信息并进行批量安装

conda search PACKAGENAME #使用conda搜索查询某个软件包，如果本机已经安装，会以“*”标识

conda info --envs #查看创建的所有“虚拟环境”
source activate bunnies #快速切换到其他“虚拟环境” 
conda create --name flowers --clone snowflakes #复制某个“虚拟环境”
```

##
```
Conda会把python本身当成一个package，就如同Python的众多package一样，conda search --full-name python可以找到所有现存python版本

假设一个应用场景：我们需使用某个第三方包，但该包依赖于比较old的一个Python版本，这时我们可以这么做：conda create --name snakes python=2.3

在某个虚拟环境中安装一个第三方包：conda install --name bunnies beautifulsoup4；经测试如果使用python2.7 -m pip install packages时，所有虚拟环境中的python2.7将都可以使用
```
## 利用源码压缩文件安装
```python
## 从https://anaconda.org/bioconda 频道中下载压缩包
## 然后把这些解压缩文件全部拷贝到conda下
cp -r * ~/miniconda2/
## 虚拟环境
cp -r * ~/miniconda2/envs/biostar/
```
## BioContainer
## Reference_Info
http://mp.weixin.qq.com/s/nK1Kkf9lfZStoX25Y7SzHQ  
https://zhuanlan.zhihu.com/p/25085567  
http://www.biotrainee.com/thread-1901-1-1.html  
什么是虚拟环境：http://blog.csdn.net/pipisorry/article/details/39998317  
https://mp.weixin.qq.com/s/1HOQ8J2vs-lEwdVAxeNiyQ
http://mp.weixin.qq.com/s/L_QdPSmwqIhpp79KEP7Shg