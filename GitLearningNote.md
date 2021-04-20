## 1.区分三个区：
工作区、暂存区和版本库，需要先将工作区加到暂存区，然后再提交到版本库。
## 2. Git常用操作

```shell
-------------------------------------------------
初始化的时候应该先配置git用户信息
git config user.name "ZhangYuanlin"
git config user.email "bhzhangyuanlin@163.com"
-------------------------------------------------

git init 初始化一个仓库
git init --bare 新建一个裸库，没有工作区，一般中央服务器是这个库

git add filename 将工作区的一个文件加到暂存区
git commit -m "xxxxx" 含有提交信息的这样提交，一般交互式的界面很少这样用，只有脚本提交才这样用。

git commit 直接这样，然后会要求提交一个说明，对于Git来说，每一个提交都应该有一个说明，说明取决于公司或者项目规范，一般第一行写一个小摘要，空一行写具体实现的内容

git log 可以查看之前的说明

推送改动
git remote add origin ttps://github.com/bhzhangyuanlin/VnoteSync.git 这里是先要初始化远端服务器地址，通过remote add添加远端服务器，origin是给远端服务器命名，默认就是origin

git remote -v 可以查看当前远端服务器地址
git push origin HEAD:master 通过push推送上去，origin表示地址，HEAD表示当前本地的点，master表示提交到远端master分支。

已经有了一个远端的库，把远端库的文件克隆下来
走https协议：git clone url 需要用户名和账号密码的验证
走ssh协议：ssh协议要求输入公钥和私钥进行验证，需要现在远端库进行设置，设置一个ssh公钥，还要设置一个私钥。一般github，gitee会有如何设置公钥，公钥记得添加到账户下，而不是具体的项目下。使用指令在电脑上生成了公钥之后，添加到远端服务器上，也会生成私钥，私钥是id_rsa，公钥是.pub扩展名那个。

在远端服务器设置了公钥之后，那么你的本地的私钥和公钥就已经匹配了，那么久不需要密码验证，就可以remote add了
```
## 3. 操作暂存区

```shell
   git add . 表示将所有的文件加到暂存区
   git rm --cache filename 将暂存区中的filename文件进行删除
   
```

   <img src="C:\Users\bhzha\AppData\Roaming\Typora\typora-user-images\image-20210420172402449.png" alt="image-20210420172402449" style="zoom:80%;" />

## 4. 查看工作区状态

```shell
git status 表示查看当前git的状态，会告诉你在哪个分支，会显示当前的工作区的待提交的内容，就是当前工作去和暂存区对于版本库的差别在哪里
文件是红色的表示在工作区，绿色的在暂存区

git status -u 有3个参数: no   normal   all 
no  只显示已经跟踪的文件
normal 显示没有被跟踪的文件和文件夹
all 未跟踪的文件夹内的文件独立显示
（未跟踪的文件是新加进去的，这里会有一些临时文件啊，测试文件啊，什么的，乱七八糟的东西，我们其实多数情况下只是关注已经跟踪的文件，所以，
使用git status -uno 来只关注已经跟踪的文件）

对于有未被跟踪的文件夹存在的情况下，git status 只会显示出文件夹名字，不会显示具体的里边有什么，需要使用git status -uall

对于git add 也有git add -u 只会加入已经跟踪的文件，不会加入未跟踪（untracked files)的文件

```

## 5. 浏览历史

```
git log 显示所有
git log -n 显示n条
git log --stat 显示修改的行数  支持git log -n --stat同时使用
git log --since=2.days 近2天的改动
git log --since=2008-01-15 从08年1月15日开始的改动
git log --until=2008-01-15 08年1月15日之前的改动
git log --author=BeginMan 只筛选出部分作者修改的信息
git log --grep=init 包含init的提交 init可以替换成你想查找的任何字符串，可以找到包含init字符串的提交，就是要之前认真写commit message
git log --pretty=oneline 单行显示
git log --pretty=format:"%h-%an,%ar:%s" 自定义格式，选项参考下面的内容
git show commit-id 可以查看某个哈希值下的具体的改动的内容
```

<img src="C:\Users\bhzha\AppData\Roaming\Typora\typora-user-images\image-20210420180417071.png" alt="image-20210420180417071" style="zoom:150%;" />

## 6. Git 常用命令 操作文件
   * git config 配置基本信息

```
使用global参数，就是对本电脑进配置，对于特定的工程若没有特定的配置，都采用这个配置
git config --global user.name "GoodBoy"
git config --global user.email "goodboy@163.com" 

使用local参数，只对本工程有用，不是对全局有用
git config --local user.name "xxxxx"

获取用户姓名和账户
git config --get user.name
git config --global --get user.name
git config --local --get user.email
```
   * git add 添加文件到暂存区

```shell
git add filename
git add .
git add -u
git add -i 是交互式的添加方法
```

* 提交commit

```shell
git commit
git commit -a 是将add和commit合并在一起执行了，会执行提交已经跟踪的文件，未跟踪的不会提交
git commit -m "msg"
```

* 比较两次提交之间文件的修改

```shell
git diff HEAD~1 HEAD
```

* 在工作区搜索

```shell
git grep -n content 在工作区找包含content的文件，不加-n不会给出行号，不会找untracking file
git grep --untracked -n content 在工作区找，不会找untracking file
git greo -n -i content -i参数表示不区分大小写
```

* 在历史中搜索

```shell
git log -G 在历史修改行中查找（每个历史的patch都会查找出来）
git log -S 字符串命中次数的变化，变化了会找出来，比如说原本是 abs, 当改成了“abs”他是不会查出来的，sba不知道会不会查出来
```

* 删除和移动文件

```shell
git rm filename 删除某个文件
git mv filename new_filename 可以重命名文件，具体的移动我觉得需要后期再学习
```

## 7. Git 常用命令分支

* 查看分支

```shell
git branch 
git branch -avv  a表示显示所有的分支， vv表示显示详细的分支信息
```

* 新建分支

```shell
git branch branchname 新建一个分支
git checkout branchname 切换到这个分支上
git checkout -b branchname 新建并且直接切换到这个分支上
```

* 删除分支

```shell
git branch -D branchname 删除分支，但是不能删除当前自己所在的分支，要先切换到其他分支，在删除要删除的分支
```

* 推送分支

```
git push origin HEAD:branch 建立远端分支
git push origin :branch 删除远端分支
```

* rebase分支

```shell
git checkout branchname 先切换到某个分支
git rebase master 将某个分支合并到主分支后面，具体看下图，会重新提交，哈希值不一样
```

![image-20210420220145954](C:\Users\bhzha\AppData\Roaming\Typora\typora-user-images\image-20210420220145954.png)

## 8. git常用命令 修改历史

* 回滚一个提交

```shell
git revert commit-id 通过git log可以查看之前的提交的commit-id，这里的commit-id是应该是想要回滚的那个id
```

* cherry-pick

```shell
git cherry-pick branchname 比如下图将上面那个分支的内容拿到下面分支后面，就应该先切换到下面分支，然后运行 git cherry-pick 上面分支名字，这里两个分支的哈希值不一样，是重新提交的
```

![image-20210420221404166](C:\Users\bhzha\AppData\Roaming\Typora\typora-user-images\image-20210420221404166.png)

* 修改前一个提交

```shell
就是先修改要修改的内容，然后使用
git add xxxxx
git commit --amend 来修改刚刚提交的内容，不会产生新的提交
可以通过git commit --amend --author="Author Name <email@address.com>"修改作者信息

备注：如果这个commit已经push到远端服务器了，那么不能被再被push。所以一定要确定好在push
```

* 修改前几个提交

```shell
使用git rebase -i 可以修改历史中的提交，可以合并删除之前的提交

git rebase -i 哈希值 这里的哈希值应该是你希望修改的那次提交之前的提交的哈希值
```

## 9. git 常用命令 技巧与实操

* 谁写坏了代码

```shell
git blame filename 可以显示每一行都是谁修改的，时间

git blame filename -L a,b 指定行数（默认情况是所有的行都显示）
```



* 找回丢失的提交（ --amend后后悔了，觉得老版本好，找回）

```shell
git reflog 默认大部分reflog可以保留3个月

所以就是现在你要找回的那个分支，然后运行git reflog，他会显示出来你的历史的各个修改的节点，找到要回到的那个节点前面的哈希值复制
然后直接删除master分支，git branch -D master
把当前的这个点叫做master, git branch master,然后切换回master: git checkout master,就变成原来的样子了
```

* git库瘦身

```
一般虽然我们删除了分支了，但是只是把分支删了，实际分支的内容还会保留，一般对于一些不可达的悬挂点，系统30天后过期，可达的点90天后过期，瘦身呢就是手动过期，手动清空
 手动过期: git reflog --expire-unreachable=now --all
 手动清理: git gc --purne=now --aggressive
日常使用功能过程中，一般好久才清理一次，所以其实没有必要手动过期，直接执行下面的那一条就好了
```

* 使用图形界面

```shell
自带的有一个 gitk 运行就打开了

还有一个IntelLiJ IDEA软件
```

