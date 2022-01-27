# git常用命令

## 下载安装

从git官网下载安装包，安装完毕后就可以使用命令行的 git 工具，在开始菜单里找到"Git"->"Git Bash"，会弹出 Git 命令窗口，你可以在该窗口进行 Git 相关命令行的操作。

```js
具体可参考https://www.runoob.com/git/git-install-setup.html
```

## 全局配置环境

配置个人用户名和电子邮箱

```js
git config –globle user.name “runoob”
git config –globle user.email text@runoob.com
```

配置完毕后，可以通过$ git config –list命令查看所有的配置信息。
也可直接查询某个环境变量的信息。

```js
git config user.name
git config user.email
```

## 查看工作区状态

```js
git status
```

- 状态一：修改了没有添加到缓存区（红色），此时可以通过git diff 查看修改了的内容，“-”号是修改前，“+”号是修改后，第一个加号后修改的前一行。第二个加号是修改的内容。
- 状态二：修改了添加到缓存区（绿色）
- 状态三：On branch master nothing to commit, work tree clean 表明无修改内容

## 添加文件到git仓库

分两步：
把修改的修改添加到版本库里的暂存区，可以单独添加某个文件，可多次使用

```js
git add <file>
```

把暂存区的所有内容提交到当前分支，提交的说明一定要写（字符串加双引号）

```js
git commit -m <message>
```

## 本地同步更新远程分支

```js
git pull
```

如果项目是多人合作的，那么就需要在拉去别人更新的代码合并到本地。Git会自动合并本地代码。

## 把缓存中的代码推送到远程分支

```js
git push
```

## 撤销修改

- 场景一：修改了文件但是未被add

```js
git checkout -- <file>
```

- 场景二：修改了工作区内容，还添加到了暂存区时，想丢弃修改，分两步

```js
git reset HEAD <file> 就回到了场景一
git checkout -- <file>
```

- 场景三：修改文件已被commit,但是没有推送到远程库，想要撤销本次提交，只能切换版本

```js
git reset --hard HEAD^
```

## 从远程分支拉取项目

```js
git clone SSH/HTTPS地址 -b <分支名>
```

## 分支管理

当前分支作业时

```js
1)查看分支：git branch
2)创建分支：git branch <name>
3)切换分支：git checkout <name>或者git switch <name>
4)创建+切换分支：git checkout -b <name> 或者 git switch -c <name>
5)合并某分支到当前分支：git merge <name>
6)删除分支git branch -d <name>
```

临时切换分支作业时

```js
1)暂存分支工作状态： git stash
2)查看分支存储的工作状态： git stash list
3)恢复分支工作状态： git stash apply
4)删除分支存储的工作状态：git stash drop
5)恢复并删除分支存储工作状态：git stash pop
```

```js
git fetch origin branch2(分支名),抓取远程分支改变
git merge origin/branch2

查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream to origin branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
```

## 如果提示没有公钥，请看[这个](https://blog.csdn.net/weixin_30763455/article/details/99901544)

## submododule 使用

在主项目中，使用 `git submodule add <submodule_url>` 命令可以在项目中创建一个子模块。

拉取主项目时 如果希望子模块代码也获取到，一种方式是在克隆主项目的时候带上参数 `--recurse-submodules`

另外一种可行的方式是，在当前主项目中执行：

> git submodule init
> git submodule update

在不同场景下子模块的更新方式如下：

- 对于子模块，只需要管理好自己的版本，并推送到[远程分支](https://www.zhihu.com/search?q=远程分支&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A87053283})即可；

- 对于父模块，若子模块版本信息未提交，需要更新子模块目录下的代码，并执行 `commit` 操作提交子模块版本信息；当主项目的子项目特别多时，可能会不太方便，此时可以使用 `git submodule` 的一个命令 `foreach` 执行：

  > git submodule foreach 'git pull origin master'

- 对于父模块，若子模块版本信息已提交，需要使用 `git submodule update` ，Git 会自动根据子模块版本信息更新所有子模块目录的相关代码。

使用 `git submodule deinit` 命令卸载一个子模块

