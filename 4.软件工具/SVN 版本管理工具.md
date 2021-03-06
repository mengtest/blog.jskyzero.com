---
layout: post
title: "工具速览：Apache Subversion"
header-img: "img/site/bg2.jpg"
tags:
    - Tutorial
    - SVN
---

本文将带你快速上手SVN，我们希望您有基本的学习能力，最好有过一些编程经验。

# SVN 版本管理工具
`jskyzero` `2018/05/20`


## 概念介绍

+ 什么是SVN
  + 通俗的来讲，SVN是一个版本管理系统(version control system, VCS)。
  + 我们经常使用的另一个版本管理工具——Git，SVN干的就和Git干的事情差不多。
+ SVN与Git的区别
  + 除了用来干的事情的目的类似以外，其他基本没什么相同。
  + 其实从用户角度工作的流程也是很类似的。
  + 其他的相同和不同，或者说优点和缺点建议自行把握。
+ SVN（和Git）的优缺点
  + 参考上个问题最后一点。

聊完了SVN我们来聊一下通用的一些概念。

+ Repository：这个单词似乎直接理解起来是仓库一类的意思，这里就是指源代码存放的地方。
+ Checkout：第一次发现这个词原来是提取的意思，如果使用过Git的话对这个单词应该不会陌生。
+ ...

## Learn by Doing

下载和安装就不再赘述，Windows的话记得检查二进制执行文件的路径是否添加到了系统Path上。

### 初始化一个仓库
假设当前路径为`D:\SVN\`：
```powershell
# 新建文件夹
mkdir hello
# 初始化
svnadmin.exe create hello
```

接下来对于目的仓库的引用，我们将采用文件路径的形式，而不再对SVN或者HTTP等需要配置的进一步的展开。

### 提取并开始工作
```powershell
# 新建目录1
mkdir 1
# 新建目录2
mkdir 2
# 提取文件副本到目录1
# 关于 file:///D:\SVN\hello
# 这个就是那啥我们上面说的以文件夹的形式引用仓库
svn chekcout file:///D:\SVN\hello 1
# 提取文件副本到目录2
svn chekcout file:///D:\SVN\hello 2
# 创建新的文件夹
cd 1
mkdir branches
mkdir trunk
mkdir tags
# 查看当前状态
svn status
# 添加文件夹
svn add *
# 再次查看当前状态
svn status
# 提交
svn commit -m "initial hello proj structure"

# 更新2
cd ../2
svn update

# 此时我们可以看到已经可以取下来新建的文件夹。 
```

### 处理冲突
那我们来手动自己制造一点冲突：
```powershell
# 制造比对对象
cd ../1/trunk
echo "hello world!" >> hello.txt
# 提交
svn add hello.txt
svn commit -m "add hello.txt"
# 制造冲突
cd ../../2/trunk
# 冲突出现
echo "hello world!!" >> hello.txt
svn add hello.txt
svn commit -m "this will fail"

# 解决冲突
svn update
# ...
# 具体的解决方法有很多，命令行交互中也提供基本的MENU里面很多选项，这里不再废话
```

## 工作流程

这个是`svn-book`中推荐的`work cycle`，其实和git的是类似的。

1. 更新当前副本，`svn update`了解一下。
2. 更改工作，可能大部分时间都在这里。
3. 检查更改，`svn status`等指令查看修改内容。
4. 改正错误，如果有错误。有丢弃修改的指令，建议svn help看一下。
5. 提交修改，当然，如果此时又冲突的话，要处理冲突。

总体来说，对于任何版本管理工具这套思维模式都是不错的。

## 补充，关于github与svn

请参考[Support for Subversion clients](https://help.github.com/articles/support-for-subversion-clients/)。

实际上试了下因为是一直是git sync的思维所以用起来多少还是有点问题，不过还可以凑合。

如果是练手或者目录不复杂的可以试试看。

## 更深的地方

### 物尽其用

+ 我们之间建了几个文件夹，这些文件夹其实都是有通用默许的推荐的使用方法的。
+ SVN对于分支的管理方式，合并，等等是怎样的流程。
+ SVN的Tag又是干什么的。
+ 当文件传输意外终端，应该怎么办？
+ 怎样对文件加锁？

等等，类似这样的问题，如果像我一样懒的话，当然是等到用到的时候再说。

### 实现背后

我们可以打开最开始创建的那个文件夹，仔细分析一波里面的组织结构和内容，SVN背后是怎样储存文件的呢？

## 参考

1. 伪装是SVN官网的超链接
2. 伪装是SVN官网文档中的某本小参考书
3. ...