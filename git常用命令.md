1.初始化本地仓库


git init
1
git init
2.添加文件到本地仓库暂存区


git add a.txt
1
git add a.txt
3.添加文件到本地仓库


git commit -m 'v1'
1
git commit -m 'v1'
此命令代表确认提交到本地仓库。-m ‘v1’代表为此添加一个版本标记v1

4.查看当前git的状态


git status
1
git status
结果A：


On branch master
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)

modified: readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
1
2
3
4
5
6
7
8
On branch master
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
 
modified: readme.txt
 
no changes added to commit (use "git add" and/or "git commit -a")
证明当前有文件已经修改，但是没有准备提交的修改，没有add和commit

结果B：


 On branch master
 Changes to be committed:
 (use "git reset HEAD <file>..." to unstage)

 modified: readme.txt
1
2
3
4
5
 On branch master
 Changes to be committed:
 (use "git reset HEAD <file>..." to unstage)
 
 modified: readme.txt
证明当前已经有文件提交了，但是还没有commit

结果C：


On branch master
nothing to commit, working directory clean
1
2
On branch master
nothing to commit, working directory clean
证明当前所有文件已经提交到本地仓库，工作目录是干净的

5.查看git日志


git log
1
git log
结果：


commit aca3fe6cc3f49ded922d05e4774561f33697f710
Author: Qingcai Cui <1016903103@qq.com>
Date: Sun Nov 2 12:16:25 2014 +0800

v2

commit 90eea044b6da3818770ec482df98bb05ab569472
Author: Qingcai Cui <1016903103@qq.com>
Date: Sun Nov 2 12:07:46 2014 +0800

v1
1
2
3
4
5
6
7
8
9
10
11
commit aca3fe6cc3f49ded922d05e4774561f33697f710
Author: Qingcai Cui <1016903103@qq.com>
Date: Sun Nov 2 12:16:25 2014 +0800
 
v2
 
commit 90eea044b6da3818770ec482df98bb05ab569472
Author: Qingcai Cui <1016903103@qq.com>
Date: Sun Nov 2 12:07:46 2014 +0800
 
v1
证明当前我们已经commit了两次，上面的为最近提交的。

如果嫌输出太多可以尝试下面的命令，一行显示


git log --pretty=oneline
1
git log --pretty=oneline
结果如下：


aca3fe6cc3f49ded922d05e4774561f33697f710 v2
90eea044b6da3818770ec482df98bb05ab569472 v1
1
2
aca3fe6cc3f49ded922d05e4774561f33697f710 v2
90eea044b6da3818770ec482df98bb05ab569472 v1
一大串类似的


aca3fe6cc3f49ded922d05e4774561f33697f710
1
aca3fe6cc3f49ded922d05e4774561f33697f710
是commit id（版本号），和SVN不一样，Git的 commit id 不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的commit id和我的肯定不一样，以你自己的为准。为什么commit id需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

6.版本回退

回退到上一版本：


git reset --hard HEAD^
1
git reset --hard HEAD^
回退到上上个版本：


git reset --hard HEAD^^
1
git reset --hard HEAD^^
如果回退的版本过多则不用加那么多的^号

比如回退到上10版本，则可以用下面的命令


git reset --hard HEAD~10
1
git reset --hard HEAD~10
回退1个版本相当于


git reset --hard HEAD~1
1
git reset --hard HEAD~1
不过现在利用git status来查看已经看不到刚才那个版本了，想要返回的话怎么办

可以仍然用上面的方法


git reset --hard aca3f
1
git reset --hard aca3f
只要写前几位就好了，git会自动去匹配的。当然前提是你的命令行窗口没有关闭还能找到之前的commit id。

不过万一你的命令行关闭了也没关系，git提供了一个方法来记录你的每一次命令。


git reflog
1
git reflog
结果：


aca3fe6 HEAD@{0}: reset: moving to aca3f
90eea04 HEAD@{1}: reset: moving to HEAD^
aca3fe6 HEAD@{2}: commit: jj
90eea04 HEAD@{3}: commit (initial): aa
1
2
3
4
aca3fe6 HEAD@{0}: reset: moving to aca3f
90eea04 HEAD@{1}: reset: moving to HEAD^
aca3fe6 HEAD@{2}: commit: jj
90eea04 HEAD@{3}: commit (initial): aa
在前方仍然显示了版本号，你仍然可以找到。仍然可以利用的reset命令来还原。

注意：

A 工作区和暂存区的名词区别

工作区：就是你在电脑里能看到的目录

暂存区：在.git文件夹下存在一个暂存区stage和好多个分支比如master

当执行git add命令时，工作区的内容便会到stage暂存区中，当执行git commit命令时暂存区的内容便会提交到分支里面。

B “Git管理的是修改”的意思

比如第一次修改readme.txt，然后执行git add到暂存区，然后再修改readme.txt，然后执行git commit 到分支。

结果调用 git status 时发现现在仍然有一个modified文件，这是因为我们没有把新修改的文件提交到暂存区，所以导致分支中的文件和在工作区的原文不匹配。所以我们需要重新add和commit。这说明git管理的是”修改”,而不是”文件”本身。

7.撤销修改


git checkout -- filename
1
git checkout -- filename
比如 git checkout — readme.txt 它的作用如下：

把 readme.txt 文件在工作区的修改全部撤销，这里有两种情况：

一种是 readme.txt 自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是 readme.txt 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次 git commit 或 git add 时的状态。

那么上面的这个情况是我们提交或者没提交到暂存区之后又对源文件做的修改。

还有另一种情况，我们提交之后放到了暂存区，我们想把暂存区里面的文件撤销回来。

就用以下命令：


git reset HEAD readme.txt
1
git reset HEAD readme.txt
就是将刚才的git add命令撤销，把暂存区中的内容撤销回到工作区。

当然，如果你不但add了，并且又commit了，那就只能进行版本回退了。在上面已经说过了。

总结：

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout — file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，进行版本回退，不过前提是没有推送到远程库。

8.删除文件

假如现在你新建了一个 hello.txt 文件,你已经add并commit到了本地分支之中。

现在你想删除，如果直接执行


rm hello.txt
1
rm hello.txt
则是只把工作区中的文件删除了，本地分支中没有删除，现在你执行git status 则会提示当前工作区已删除了一个文件，本地分支仍然存在，就存在了工作区和本地分支不同步的问题，现在我们如果想恢复一下，就利用下面的命令


git checkout -- hello.txt
1
git checkout -- hello.txt
将本地的文件一键还原。

假如我们真的是想将本地分支中的一个文件删除，那么我们就执行下面的方法。


git rm hello.txt
1
git rm hello.txt
执行了这个命令之后，我们执行 git status 之后就会提示 你现在需要commit一下确认删除。

并且执行完这个命令之后，我们执行 ls 命令之后发现本地的文件也已经不存在了。

如果有很多个文件在本地被删除掉了，暂存区和工作区的文件完全不一样了，工作区删除掉了很多个文件，就会出现


Changes not staged for commit:
deleted: a.txt
deleted: b.txt
1
2
3
Changes not staged for commit:
deleted: a.txt
deleted: b.txt
如果一个个地删除文件肯定特别麻烦，所以我们可以用下面的命令来


<span class="hljs-title">git</span> add -A
1
<span class="hljs-title">git</span> add -A
它等同于

git add . 和 git add -u


git add . 保存所有新文件和改动的文件，不包括删除的文件
1
git add . 保存所有新文件和改动的文件，不包括删除的文件

git add -u  保存所有改动的文件和删除的文件，不包括新的文件
1
git add -u  保存所有改动的文件和删除的文件，不包括新的文件
git add -A 和 git add -u 会把我们未通过 git rm 删除的文件全部stage 使用过以后运行 git status 便会出现 changes to be committed … 说明已经都经过git rm 删除了. 综上，我们想删除分支中的文件时就需要两条命令合起来使用


git rm hello.txt
git commit -m 'rm hello.txt'
1
2
git rm hello.txt
git commit -m 'rm hello.txt'
要小心的是，执行 git rm 方法之后工作区和本地分支中的文件已经都不存在了。所以如果你已经在远程仓库存在备份的话，你就不用担心误删了。否则你就要小心了，会丢失这个文件的。 对比修改的文件


git diff
1
git diff
结果：


diff --git a/readme.txt b/readme.txt
index 9c58532..8ce5f7e 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1 @@
-git is grea
+hello is grea
1
2
3
4
5
6
7
diff --git a/readme.txt b/readme.txt
index 9c58532..8ce5f7e 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1 @@
-git is grea
+hello is grea
-号开头的语句代表这句话删除掉了 +号开头的语句代表这句话为新增语句 9.分支 新建本地分支


git branch dev
1
git branch dev
切换到dev分支


git checkout dev
1
git checkout dev
创建并切换到该分支


git checkout -b dev
1
git checkout -b dev
查看本地分支


git branch
1
git branch
比如此命令会输出 * master hello 代表当前已经选中了master分支，存在本地两个分支master和hello 合并分支


git branch master
git merge hello
1
2
git branch master
git merge hello
这样我们就把 master 分支合并到了hello分支了。 接下来我们就可以删除本地分支了。 既然分支合并这么简单，它可不可能出现什么问题呢？当然有，当然会出现分支合并冲突的问题。 比如我切换到了hello分支并修改了一个文件 add并且commit，然后我切换回了master分支 同样修改了这个文件，add并且commit。现在我们如果执行git merge hello 则会报一个提示说 分支合并冲突。 我们查看刚才修改的文件发现已经发生了变化，我们需要手动修改完了之后，然后继续add和commit才可以。如果没有add和commit，那么我们无法切换回原来的分支的。 add和commit之后，原来的hello分支仍然保持了原来不变，只不过是我们的master分支改变了。下面示意图则清楚地表示出了如下关系，图中的feature1相当于hello分支。 合并之后，我们就可以进行删除分支了，可以删除掉hello分支。 0 其实上面的一系列操作 从merge到修改 然后add 然后commit 其实可以利用一条命令来修改,这个方法叫禁用Fast Forward模式 刚才的一系列命令如下


git checkout master
git merge hello
vi file.txt
git add file.txt
git commit -m 'merge master'
1
2
3
4
5
git checkout master
git merge hello
vi file.txt
git add file.txt
git commit -m 'merge master'
那么利用下面的语句同样可以达成同样的效果


git checkout master
git merge --no-ff -m 'merge master' hello
1
2
git checkout master
git merge --no-ff -m 'merge master' hello
因为本次合并要创建一个新的commit，所以加上 -m 参数，把commit描述写进去。 Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。 结束之后我们照样可以删除hello分支。 删除本地分支


git branch -d hello
1
git branch -d hello
查看远程分支


git branch -r
1
git branch -r
查看本地和远程分支


git branch -a
1
git branch -a
现在遇到一个问题，我们正在工作的一个分支还没有做完，不能add或者commit，但是现在有一个bug需要在另一个分支上去修复，那么我们就需要暂时存储当前的分支。所以就要用到下面的命令


git stash
1
git stash
当我们处理完其他的事情之后，再切换回来此分支。 先使用


git stash list
1
git stash list
再取出stash列表中的内容


git stash pop
1
git stash pop
上面的方法既取出了list内容，并且把list中的元素删除掉。 强行删除分支


git branch -D new-branch
1
git branch -D new-branch
10.多人协作 多人协作的工作模式通常是这样： 首先，可以试图用


git push origin branch-name
1
git push origin branch-name
推送自己的修改； 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并； 如果合并有冲突，则解决冲突，并在本地提交； 没有冲突或者解决掉冲突后，再用


git push origin branch-name
1
git push origin branch-name
推送就能成功！ 如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令


git branch --set-upstream branch-name origin/branch-name
1
git branch --set-upstream branch-name origin/branch-name
这就是多人协作的工作模式，一旦熟悉了，就非常简单。 11.添加远程地址别名


<script src="//www.google-analytics.com/analytics.js" async=""></script>git remote add origin git@github.com
1
<script src="//www.google-analytics.com/analytics.js" async=""></script>git remote add origin git@github.com
删除远程地址别名


git remote rm origin
1
git remote rm origin
此方法删除掉origin这个地址别名

查看远程地址别名


git remote -v
1
git remote -v
推送到远程分支

我们在github上新建一个项目

添加远程仓库


git remote add origin  git@github.com:cqcre/gittest.git
1
git remote add origin  git@github.com:cqcre/gittest.git
推送上去


git push -u origin master
1
git push -u origin master
加上了 -u 参数，Git不但会把本地的 master 分支内容推送的远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来，即相当于推送到了一个默认的分支，在以后的推送或者拉取时就可以简化命令。

那么以后我们就不需要关联了，以后推送的话我们只需要输入


git push origin master
1
git push origin master
克隆远程仓库

假设现在已经存在了一个远程仓库，我们需要把这个仓库克隆到本地，我们需要使用下面的方法


git clone git@github.com:cqcre/test.git
1
git clone git@github.com:cqcre/test.git
这样我们就把远程的一个仓库取到了本地了。

12.强制操作

强制覆盖本地分支内容


git fetch --all
git reset --hard origin/master
1
2
git fetch --all
git reset --hard origin/master
强制覆盖远程内容


git push origin master --force
1
git push origin master --force
13.修改commit的备注

有时候我们commit的备注写错了，需要重新修改，可以利用如下命令


git commit --amend
1
git commit --amend
14. .gitignore的使用

在git中如果想忽略掉某个文件，不让这个文件提交到版本库中，可以使用修改根目录中 .gitignore 文件的方法（如无，则需自己手工建立此文件）。这个文件每一行保存了一个匹配的规则例如：


*.a # 忽略所有 .a 结尾的文件
 !lib.a # 但 lib.a 除外
 /TODO # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
 build/ # 忽略 build/ 目录下的所有文件
 doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
1
2
3
4
5
*.a # 忽略所有 .a 结尾的文件
 !lib.a # 但 lib.a 除外
 /TODO # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
 build/ # 忽略 build/ 目录下的所有文件
 doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
规则很简单，不做过多解释，但是有时候在项目开发过程中，突然心血来潮想把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：


 git rm -r --cached .
 git add .
 git commit -m 'update .gitignore'
1
2
3
 git rm -r --cached .
 git add .
 git commit -m 'update .gitignore'
15. 打tag

在发布版本的时候，经常会用到打标签的方法。

增加一个标签


git tag -a "v1.0.0" -m "Version 1.0.0"
1
git tag -a "v1.0.0" -m "Version 1.0.0"
删除一个标签


git tag -d v1.0.0
1
git tag -d v1.0.0
推送所有标签


git push origin --tags
1
git push origin --tags