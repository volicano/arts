## ARTS

### Share

## Git常用命令

1、git init  在一个目录中执行这个语句可以将该目录变成git的版本库（即git仓库）。

2、把一个文件放到Git仓库只需要两步：

1）git add 文件名               （把文件修改添加到暂存区；）

2）git commit -m "描述信息"      （把暂存区的所有内容提交到当前分支。）

注：可以多次add一次提交，也可以一次add多个文件

3、git status 查看仓库当前状态

注：从来没有被添加过的文件状态是Untracked。

4、git diff readme.txt：查看readme.txt修改了什么内容

5、git log：显示从最近到最远的提交日志

如果觉得这个命令输出信息太多，可以用命令：git log --pretty=oneline

6、HEAD：当前活跃分支的游标，你现在在哪儿，HEAD 就指向哪儿，所以 Git 才知道你在那儿！不过 HEAD 并非只能指向分支的最顶端，它可以指向任何一个节点，它就是 Git 内部用来追踪当前位置的指针。

7、git reset --hard HEAD^：将当前版本回退到上一个版本。（HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写成HEAD~100）

注意：这里有个问题，回退到一个历史版本后，用git log命令只能查看到该历史版本之前的版本。要回到之后的那些版本可以找到目标版本的commit_id，用git reset --hard commit_id命令回到

对应的版本（commit_id可以不用写全）。那如何找到commit_id呢，如果git窗口没关的话最方便查看了，如果关了，用git reflog，可以查看我的每一次命令。

8、git reflog：查看命令历史

   git log：查看提交历史

9、git diff HEAD -- readme.txt：查看工作区和版本库里面最新版本的区别

10、git checkout -- filename：把filename文件在工作区的修改撤销到最近一次git add 或 git commit时的内容（意思是：最近操作是add，则撤销到最近一次add时的内容；如果是commit则同理）

注意：--不能少，否则就变成切换分支了

11、撤销修改的三种情况：

1）当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

2）当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

3）已经提交了不合适的修改到版本库时，想要撤销本次提交，可以回退至上一个版本，不过前提是没有推送到远程库。

12、删除文件：

1）工作区文件删除，直接在文件管理器中把没用的文件删了，或者用rm命令删了。

2）这里分两种情况：一种确实要删除，则依次执行以下两个命令，git rm 文件名，git commit -m "描述"；另一种情况是删错了，则直接从版本库中还原，git checkout -- 文件名

13、git remote add origin 远程库地址：将本地仓库关联上远程仓库并将远程仓库取名为origin，注意要先在远程库添加公钥才可以从本地往远程推送数据

14、git push -u origin master：第一次推送master分支的所有内容

15、git push origin master：推送最新修改

16、git clone 远程库地址：克隆远程仓库到本地

注：克隆后，Git自动把本地的master分支和远程的master分支对应起来了

17、git branch：查看分支

18、git branch 分支名：创建分支

19、git checkout 分支名：切换分支

20、git checkout -b 分支名：创建+切换分支

21、git merge 分支名：合并某分支到当前分支

22、git branch -d 分支名：删除分支

23、git log --graph --pretty=oneline --abbrev-commit：在合并分支后，可以用这个命令查看分支的合并情况

24、通常合并分支时，Git会用Fast forward模式，但是在删除分支后，会丢掉分支信息。如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，从分支历史上就可以看出分支信息。

在合并命令中加上--no-ff就能禁用Fast forward模式，git merge --no-ff -m "merge with no-ff" dev，因为合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

25、实际开发中分支管理策略：

1）master分支应该是非常稳定的，仅用来发布新版本，不能在上面干活，而且要时刻与远程同步。

2）干活都在dev分支上，要发布某个版本时就可把dev分支合并到master上，在master上进行发布。所有成员都需要在dev分支上面工作，所以也需要与远程同步。

3）平时开发时都在自己的分支上开发，然后往dev分支上合并。

4）bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug。

5）feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

26、git stash：把当前工作现场“储藏”起来，等以后恢复现场后继续工作，通常用于工作一半没法提交但又必须切换到其他分支进行紧急任务时。这个命令会把工作区变成干净的。

27、git stash list：查看我们之前“储藏”起来的工作现场

28、恢复工作现场，两种方法：

1）git stash apply：恢复现场，git stash drop：删除stash内容

2）git stash pop：恢复现场并删除stash内容

注意：如果“储藏室”里面有多个stash，则恢复的时候需要指定具体哪一个stash，比如git stash apply stash@{0}

29、git branch -D feature-vulcan：强行删除一个分支，比如要丢弃一个没有被合并过的分支

30、git remote：查看远程库的信息

31、git remote -v：查看远程库的更为详细的信息

32、在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

33、push冲突：当向远程推送时可能与小伙伴的最新提交有冲突，这时先用git pull把最新的提交从origin/dev拉下来，然后，在本地合并，解决冲突，再commit，再push。

34、如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

35、git push origin develop:wz_develop 将本地develop分支推送到远程wz_develop分支

36、git pull <远程库名> <远程分支名>:<本地分支名>，，从远程库中获取某个分支的更新，再与本地指定的分支进行自动merge（本地分支可以不存在），如果是合并到本地当前分支，则:<本地分支名>可省略

37、冲突解决：在feature分支上开发后，然后切换到develop分支将feature分支合并过来，发生了冲突。解决方法是根据提示手动修改冲突文件，然后add冲突文件，然后commit，以上操作步骤都是在develop分支进行。


