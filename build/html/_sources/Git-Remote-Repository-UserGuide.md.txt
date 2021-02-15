
## git remote repository Using

[reference site](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8)

### 查看远程仓库

``` bash
$ git remote  #显示仓库信息
$ git remote --version #显示对应的克隆地址
$ git remote -v   #显示对应的克隆地址
```

### 添加远程仓库

``` bash
$ git remote add pb git://github.com/paulboone/ticgit.git  #git remote add [shortname] [url]
```

### 远程仓库抓取数据

``` bash
$ git fetch pb # git fetch [remote-name]
```

### 推送数据到远程仓库

``` bash
$ git push origin master #git push [remote-name] [branch-name]
```

### 查看远程仓库信息

``` bash
$ git remote show origin  #git remote show [remote-name] 
```



