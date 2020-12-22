git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.up rebase
git config --global alias.ci commit

git config --global user.name "Jack Wang"
git config --global user.email "baofengyujue@live.com"
一下两条命令查看上面的设置
git config --global --list
git config --list

如果不设置邮箱和用户名的话(邮箱,用户名缺一不可),commit的时候会提示设置,不然commit不上去
设置过后与远程同步过后会显示该commit由设置的邮箱用户提交
也就是说邮箱的设置很关键,如果邮箱设置错了,就会出现网页上查看明明是自己的repo,自己的账号push上来的改变却显示为错误邮箱所对应的github用户所做的commit的奇葩情况
名字设置错了似乎不会影响

--global要是放到最后的话,好像不起作用

git log
git log --stat
git log --author="<pattern>"
git log --grep="<pattern>"
git log -p
git log --oneline
git log <since>..<until>
git log <file>
git log --graph --decorate --oneline

git log --author="John Smith" -p hello.py
This will display a full diff of all the changes John Smith has made to the file hello.py.

git log 3157e..5ab91
will display everything between the commits with ID's 3157e and 5ab91

3157e~1
refers to the commit before 3157e

You can view all commits across all branches by executing
git log --branches=*

git branch

git branch -a
will return a list of all known branch names

One of these branch names can then be logged using
git log <branch_name>


git log --oneline
git checkout a1e8fb5
git checkout master


 git reset
 	--mixed
 	--hard
 git clean


- Once changes have been committed they are generally permanent
- Use git checkout to move around and review the commit history
- git revert is the best tool for undoing shared public changes
- git reset is best used for undoing local private changes

shared/remote->revert
private/local->reset

git log for finding lost commits
git clean for undoing uncommitted changes
git add for modifying the staging index

It lets you combine staged changes with the previous commit instead of creating an entirely new commit

echo "# dd" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/baofengyujue/dd.git
git push -u origin master

One of the most common ways I use relative refs is to move branches around. You can directly reassign a branch to a commit with the -f option. So something like:
git branch -f master HEAD~3
git branch -f master a1(移动到a1的commit上)
这样移动会将当前分支到沿其分支线向上第一个有分支的commit间的commit删掉,然后当前分支移动到定位点

下面3句定位点相同
git reset master
git reset HEAD^
git reset C1


git revert C2
若当前分支为C1上的a,那么上句会在C1上将a分叉出来对应于一个C2的副本

reset过后,工作目录中的文件并没有还原回去,只是状态还原了,这时会提示有unstaged的文件
但是如果目的是还原,那么就应该不要add(这时add就会回到新的状态了)
而是git checkout -- a.md b.md将工作目录中旧到新的改变给舍弃


cherry-pick的时候可能会遇到文件被删除或修改部分冲突等冲突,这时就会有Unmerged path提示出现,
这时使用git add就是保留文件,git rm就是删除文件(不管是deleted by them还是deleted by us)
(rm会真的删除文件的,还不知道怎么复原)

rm -rf .git
git init

git remote rm origin
git remote add origin https://github.com/jackwang518/jackwang518.github.io.git
git remote
git remote -v

bash中两次tab没有间隔的时间限制
windows的git bash中选择文本过后不用像cmd中的右键一下即可复制

git branch -m为重命名
git commit -m为添加信息

git branch -d删除分支


git commit -a -m 'hahaha'

可以使用单引号

如果用git remote -v查看发现账户是为更换了的账户
还出现remote: Permission to baofengyujue/test.git denied to jackwang518.问题的话
那么再多git push -u origin master一两次就可以了,会再次询问账户名和密码

cls这种cmd的命令是找不到的,git bash服从的是bash指令
所以要用clear或者ctrl+L

cd /f进入f盘
~为/c/Users/JackWang


git branch -a --help
就在bash中查看各种命令的大致帮助,而不是打开浏览器中的帮助离线页(有可能省略,要查看全的就去掉-a,直接git branch --help)

在处理合并vim中填信息的时候,最上面的黄字为信息,最下面(可能第二行就可以)为详细信息

pull默认包含merge,但是如果merge的有冲突,并不会如手动merge一样报错

push会把o/master拉下来


git checkout -b aa HEAD^在上一个commit上创建名为aa的branch
git checkout -b aa C2在C2 commit上创建名为aa的branch

git rebase -i HEAD~3就是将选框中的commit排序选择后逐次在定位点开出的一个新分支上依次向下排列,最后原分支线地低端的分支转移到新分支线末尾,而原分支从最底沿上到第一个有分支的commit间的commit都删掉了
选框中的commit为定位点到当前分支的分支线上的commit,或者是定位点沿上的第一个分支线分叉处到当前分支间的commit
如果当前分支在一分支线其间,那么rebase不会删除各个commit,只会移动当前分支到新分支线最底部
如果当前状态为指向commit的HEAD状态(detatch),那么也不会删除各个commit,当前指向的commit上的分支也不会移动,移动的只有HEAD

分叉处的表述是不对的,只要分支线上还有其它分支,这条分支的分支线的commit就不会删除

git cherry-pick C1 C2将C1,C2依次放到当前commit下,而当前分支更新到最底部
如果C1等其中任何一个commit为当前分支线上的一次commit,那么会出现已经存在的报错



git commit --amend相当于git rebase -i HEAD^

git rebase b1 b2;将b2 rebase到b1上
准确的说是将c2沿其分支线向上到与c1分支线相叉处间的commit依次搬到c1分支下,而原分支从最底沿上到第一个有分支的commit间的commit都删掉了(和git rebase -i情况一样)
这里b1,b2可能为commit而不是branch,这时搬运的就只是commit了,而HEAD指向最下方的commit,原分支保持不变(原分支最下的branch没有移动和删除)
如果之前已经有一条分支线p1,rebase到另一个分支p0上了,那么原来分支线p1上分叉的另一分支p2(p1,p2存在分叉),rebase到p0时,p1和p2分支线重叠的commit就不用再次移动了,直接移动p1和p2不同的部分(分叉下来p2的部分),然后将p1和p2的所有部分删除(如果只有p1和p2的话)

git rebase master将当前分支rebase到master分支上
git rebase aa会将当前分支沿上到与aa分叉的所有commit移动到aa下

git reset HEAD~3
将当前分支移动到定位点,而原当前分支沿其分支线向上到第一个有分支间的commit删除掉
git revert HEAD~3
将定位点的commit复制到当前分支下,当前分支更新到最底部


commit在detatched状态下,只要git checkout别处就删除了

所谓的commit或分支的删除并不是真正删除,还可以通过其commit的序号找回,比如git branch -ff aa C9将aa分支强行移动到删除的commit,C9上


分支指的是分支名
删除指的是commit颜色变淡


git merge的时候当前分支会更新到底端
git merge的时候如果是merge到当前分支以下的分支,那么就相当于移动了分支

git checkout -b foo origin/master为设置foo,track,origin/master
也就是如果foo有新的commit,仍然可以push上去,作用和master一样

git branch -u o/master foo
will set the foo branch to track o/master. If foo is currently checked out you can even leave it off:
git branch -u o/master
相比checkout,branch本身就存在


git pull --rebase

git push origin master
git push origin foo^:master
git push origin master:newBranch

git push origin :foo


git commit -a -m 'created readme.md'
似乎创建的文件-a没有track


git push origin master
git push -u origin master
都有可能需要密码,也有可能不需要
多试几次,就会出来需要账户密码的提示了


Check what git remote -v returns: the account used to push to an http url is usually embedded into the remote url itself.
https://Fre123@github.com/...
If that is the case, put an url which will force Git to ask for the account to use when pushing:
git remote set-url origin https://github.com/<user>/<repo>
Or one to use the Fre1234 account:
git remote set-url origin https://Fre1234@github.com/<user>/<repo>
Also check if you installed your Git For Windows with or without a credential helper as in this question.
I finally found the solution.
Go to: Control Panel -> User Accounts -> Manage your credentials -> Windows Credentials
Under Generic Credentials there are some credentials related to Github,
Click on them and click "Remove".
That is because the default installation for Git for Windows set a Git-Credential-Manager-for-Windows.
See git config --global credential.helper output (it should be manager)



无论是git init还是git clone第一处commit的时候都要使用git add而不是git commit -a




If you added this path by mistake, you can remove it from the index with:
git rm --cached cc


Changes to be committed:
  (use "git rm --cached <file>..." to unstage)


git add --all
git add -A
上面两句等价,但是A不能改成小写a,区分大小写


如果目录下面有子目录而且子目录为本地repo,那么在目录下git init过后,然后再通过删除子目录中的.git目录使其成为正常目录,是不能够使原目录认为其是个正常目录的,也就会导致push到远程的目录为什么submodule之类的东西,通过正常add的话就会导致没有将子目录中的各个文件push上去;
要解决这个问题删除原目录的.git,再重新初始化,添加就可以了


git commit -a -m 'xx'
git commit -am 'xx'
等价


credential manager

To stop tracking a file that is currently tracked, use git rm --cached.


github desktop中,如果发现push出现验证问题,可能是本地的credentials删减将其搞糟了,这时在options中sign out再sign in就行了

在github desktop中切换账号后(options->accounts选项卡下),记得也要更改git选项卡下的信息



The workflow is simple as
- Fork the repo in github
- Clone the repo to your machine
- Make a branch and make necessary changes
- Push your changes to your fork on GitHub git push origin branch-name
- Go to your fork on GitHub to see a Compare and pull request button
--这里看不到那个按钮(如果是新开了一个branch上push的就看得到,在master上好像不会出现那个Compare and pull request按钮),需要自己在自己fork的repo上new一个pr,自动就有个和原repo比较的pr模板呈现给你
- Click on it and give necessary details

github中有趣的是非作者也能在作者repo上new一个pr(感觉不是contributor的话,这样做没什么意义,提醒作者要比较了吗?(但作者还不清楚么...),顶多是自己去比较比较不同branch的差异,但这又不是其pr的作用(比较差异自己查看commit就行了啊)),总之非contributor贡献其它作者repo需要先fork再提交,再new pr,而pr的一般打开方式也基本上如此

给作者repo提pr过后最好mention作者,或者要求其review或者其它方式提醒作者,因为作者在点击他github主页上方的pull request后,里面有的created/assigned/mentioned/request review标签栏中都不会有你的pr(如果你不做任何提醒的话),当然作者还是可以在该repo页面的pr中看到你的请求,或者邮件中可能也有通知
总之,你给作者提了pr,作者应该是能够接到消息的(无论你提没提醒),但是如果作者pr众多,事物众多,你不通过各种方式提醒或者request review的话,可能作者就会忽略/忘记你的pr了,或者你的pr一直就在作者处理pr流的底部,很长时间都不会爬升到让作者review再merge的地步



.gitignore文件改动了/删除了过后,可以再次git add并不会有什么重复添加等错误,并且这时git add会应用改动后的.gitignore文件,但是应用改动后的并不会将之前添加进cache的给移出来(因为是git add嘛),要移除指定文件需要git rm --cached <file>,这里file可以是'.'所有文件,也可以是空格分开多个文件,但是'.'所有文件时要使用-r参数git rm --cached . -r,不然不会移除任何文件(这时会提示你加上-r)

.git/info/exclude文件中的这句
git ls-files --others --exclude-from=.git/info/exclude
的作用是,显示使用该排除文件后会添加的文件
exclude文件保存过后,即可用git status看到效果

全局的排除文件需要指定路径
git config --global core.excludesFile ~/.gitignore

共享的,当前repo的,global的排除文件格式都是一样的,保存过后都可立即看到效果


git bash不区分大小写,不然输入了ty re再tab就可展开README.md

git commit中-a选项似乎并不好用,最后是用`git add .`保险些
(-a会add所有改变,而不会包含增加或减少的改变,应该是这样)

git中移动文件被称为renamed,比如将aa从A移到B文件夹为renamed: A/aa->B/aa

git push origin master和git push -u origin master是不同的,有时候git push -u origin master需要输入账户密码,但这时git push origin master却不要,但git push -u origin master第一次也不需要密码,可能其只有第一次不要密码吧


git commit -m "Title" -m "Description .........."
如果在vim中输入的话,那么在title行下空一行再输入就是description了

github中repo命名只能为英文和连字符(下划线也应该可以),其它一切都不行

git中文件最好都要在末行留为空行