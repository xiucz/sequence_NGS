
#### 怎么push到github:
```bash
git add 文件名
git commit -m "update"
git push origin master
```
#### 移动文件夹
```bash
xiucz@local:~/.ssh$ git clone git@github.com:xiucz/metagenome1.git 
xiucz@local:~/.ssh$ cd metagenome1/
xiucz@local:~/.ssh/metagenome1$ git mv Rscripts/LEfSe/ ./
xiucz@local:~/.ssh/metagenome1$ git status
xiucz@local:~/.ssh/metagenome1$ git commit -m "mv"
xiucz@local:~/.ssh/metagenome1$ git push origin master
```
