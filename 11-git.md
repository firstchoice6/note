# 0 快速上手

| 序号 | 命令         | 作用                                                         |
| ---- | ------------ | ------------------------------------------------------------ |
| 1    | git init     | 新建一个目录，将其初始化为Git代码库                          |
| 2    | git clone    | 下载一个项目和它的整个代码历史                               |
| 3    | git pull     | 取回远程仓库的变化，并与本地分支合并                         |
| 4    | git add      | 添加当前目录的所有文件到暂存区（"."号可暂存全部，也可以是文件名或目录） |
| 5    | git commit   | 提交暂存区到仓库区                                           |
| 6    | git push     | 推送代码到远程库                                             |
| 7    | git diff     | 显示暂存区和工作区的差异                                     |
| 8    | git checkout | 切换到指定分支，并更新工作区                                 |
| 9    | git fetch    | 下载远程仓库的所有变动                                       |
| 10   | git merge    | 合并指定分支到当前分支                                       |



### 1安装

淘宝镜像：https://npm.taobao.org/mirrors/git-for-windows/

### 2 本地仓库的使用

#### 2.1 工作流程

工作区 => 暂存区 => git仓库 

- 工作区
  - 添加编辑修改文件等动作
- 暂存区
  - 暂存已经修改的文件最后统一提交到git仓库
- git仓库
  - 最终确定的文件保存到仓库，成为一个新的额版本，并且对他人可见

#### 2.2 本地仓库操作

-  全局配置

  ```shell
  右键 git bash here
  $ git config -- global user.name '用户名'
  $ git config -- global user.email '邮箱'
  ```

- 创建仓库

  ```shell
  # 尽量用空目录
  # 路径不能用中文
  
  git init 
  # 创建了一个.git的隐藏目录
  ```

- git常用指令

  ```shell
  # 查看当前状态
  git status
  # 添加到缓存区
  git add 文件名
  git add 文件名1 文件名2...
  git add .   # 添加当前目录 /可以省略
  
  # 提交到版本库
  git commit-m "注释内容（可以写中文）"
  
  ```

#### 2.3 版本回退

```shell
- 查看版本
git log
git log --pretty=oneline

- 回退
git reset --hard 哈希id编号（可以不用写全，至少写4位）
# 回退之后不能看到之后的更新版本

- 回到新版
git reflog # 得到commit id
git reset --hard 哈希id编号
```

### 3 远程仓库

#### 3.1 线上仓库创建

github

#### 3.2 基于HTTP协议

```shell
1. 创建空目录shop

2. 使用clone指令克隆

git clone https://github.com/.....

3. 操作
 提交暂存区
    git add ...
 提交本地仓库
     git commit -m "注释"
 提交线上仓库
    # 首次往线上仓库shop提交内容的时候，必须需鉴权
    # 路径 ./git/config
    # 修改 url = https://id:password@github.com/.....
    git push # 提交到github
 拉取线上仓库
 	git pull 
```

#### 3.3 基于ssh协议

该方式与前面https方式相比，只是影响gb对于用户的身份鉴权方式，对于git的具体

操作（如提交本地、添加注释、提交远程等操作）没有任何影响。

生成公私玥对指令（需先自行安装OpenssH）：ssh-keygen-trsa-C“注册邮箱"

步骤：

- 打开提示

- 创建公私玥对文件
- 上传公钥文件内容
- 执行后续操作

### 4 分支管理

```shell
查看分支 （默认只有一个master分支）
git branch
创建分支
git branch 分支名
切换分支
git checkout 分支名
# 对于新分支 可以使用git checkout -b 创建并切换
删除分支
git branch -d 分支名
合并分支
git merge 被合并的分支名

```

### 5 冲突的产生于解决

```shell
- 先pull
git pull
# 此时git已经将线上与本地仓库的冲突合并到了对应的文件中

- 解决冲突， 重新提交
git add
git commit -m "解决了冲突"

```
### 6 忽略文件
场景：在项目目录下有很多万年不变的文件目录，例如ss、is、images等，或者还有一些目录即便有改动，我们也不想让其提交到远程仓库的文档，此时我们可以使用“忽略文件”机制来实现需求。

忽略文件需要新建一个名为`.gitignore`的文件，该文件用于声明忽略文件或不忽略文件的规则，规则对当前目录及其子目录生效。

***注意：该文件因为没有文件名，没办法直接在windows目录下直接创建，可以通过命令行Git Bash来touch创建。***

```shell
touch .gitignore
常见规则写法有如下几种：

1）/mtk/ 过滤整个文件夹

2）*.zip 过滤所有.zip文件

3）/mtk/doc 过滤某个具体文件

4) !index.php 不过滤具体某个文件

```

