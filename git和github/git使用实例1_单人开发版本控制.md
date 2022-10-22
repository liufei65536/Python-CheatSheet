# git 使用实例1_单兵作战一把梭
如果不熟悉git命令，可以到下面这个网站熟悉一下git命令：
一个交互式学习git命令的网站：
https://learngitbranching.js.org/?locale=zh_CN

————————————————————————
一个最简单的场景：
个人开发，使用github作为仓库，使用git作为版本控制。
单分支。

## 一、设置本地仓库
1. 创建文件夹，并进入
```
mkdir git-tutorial
cd git-tutorial
```
2. 初始化仓库
```
git init
```

3. 添加文件
添加README.md :
```
git add README.md
```
添加所有文件：
```
git add .
```
4. 提交更改到仓库中
```
git commit -m "第一次提交"
```

## 二、设置远程仓库
1. github上新建仓库
在github上新建一个仓库，名称最好与本地的一致。创建时不要勾选`Initialized this repositroy with a README`。
否则会和本地仓库存在冲突，需要强制覆盖。

2. 添加远程仓库
在GitHub上创建的仓库路径为“git@github.com:用户名/git-tutorial.git”
使用git remote add 命令设置它为远程仓库，并起名为origin。   
```
git remote add origin git@github.com:github-book/git-tutorial.git
```

3. 推送到远程仓库
如果想将当前分支下本地仓库中的内容推送给远程仓库，需要用到git push命令。现在假定我们在master分支下进行操作。
```
git push -u origin master
```

-u参数可以在推送的同时，将origin仓库的master分支设置为本地仓库当前分支的upstream（上游）。添加了这个参数，将来运行git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从origin的master分支获取内容，省去了另外添加参数的麻烦。
（后面直接git push即可，不需要加参数）
## 三、版本回溯
要让仓库的HEAD、暂存区、当前工作树回溯到指定状态，需要用到git rest --hard命令
```
git reset --hard 哈希值
```

状态对应的哈希值可以通过
```
git log --graph
```
查看。
