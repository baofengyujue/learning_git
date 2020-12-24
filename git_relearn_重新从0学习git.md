git_relearn.md

- [配置](#配置)
- [开始](#开始)
  - [aa](#aa)
- [Viewing project history](#viewing-project-history)
- [branch](#branch)
- [Git for collaboration](#git-for-collaboration)
- [连接GitHub](#连接github)
  - [直接通过git clone连接GitHub](#直接通过git-clone连接github)
  - [总结GitHub连接方式(https)](#总结github连接方式https)

# 配置

ubuntu下安装git
```
$ sudo apt-get install git
```

在教程进行之前，让我们先通过下面两个命令来配置一下属于我们自己的git
```
$ git config --global user.name "jack518"
$ git config --global user.email "baofengyujue@live.com"
```

执行下面命令，查看刚才的配置
```
$ git config --global --list
```

更改git在ubuntu下默认使用的nano编辑器为vim，后面commit时会用到
```
$ git config --global core.editor vim
```

# 开始

ubuntu下我的bash shell的结构：
`jack518@ubuntu:~/Documents/jw_git/p1$ ls`

- 创建目录jw_git，再在此目录中创建目录p1，p1就为我们下面git教程的根目录
- 首先我们通过vim来创建一个文件
  ```
  $ vim hello.md
  ```
- 在hello.md中第一行输入一句hello
- 保存退出后，执行以下命令，初始化git
  ```
  $ git init
  ```
- 将p1目录下所有文件的快照添加到git临时待命区(staging area)中，git将这个临时待命区称作索引index
  ```
  $ git add .
  ```
- 执行以下命令会将索引index里的内容存储起来，之前还在staging area中的快照现在staged了（上台了）
  ```
  $ git commit
  ```
这时默认会进入nano编辑器编辑此次commit信息,<br>
但因为之前已经通过命令设置过git默认编辑器，所以这里默认就是用vim来编辑commit信息

关于commit信息的编辑格式是这样的，第一行为对该次commit的总结或简单描述信息，<br>
然后留一行空行，从第三行开始可以输入一些更为详细的描述。

第一行的描述信息是必要的，详细描述是可省的

## aa

```
$ git commit -a
```
该命令会将索引中（之前add过的）的所有文件add过后再commit<br>
这个a既有all的意思，又有add的意思<br>
索引index(待命区/上台区staging area)<br>
想象一个拳击比赛，选手坐在下面一块区域准备上台，<br>
这块区域称作：<br>
staging area可以直译为上台区<br>
staged可以理解为上台了

对文件做一些更改，或者添加删除一个文件，然后执行命令：
```
$ git add .
```
将所有文件添加到索引index中

当然也可以添加指定文件如：
```
$ git add file1 file2
```

关于命令：
```
$ git diff --cached
```
对于单个文件：<br>
查看该文件在staging area中临时保存的快照(也就是文件所有add完过后时的快照，不包括对文件还未add的改变，如果还未add过，就没有差异，此命令返回空)与最近一次commit时的差异

(文件多次add后的状态会合并，比如第一次add在文件一行添加字符'a',第二次在'a'后面添加'b'，等你再用diff 查看时,这两条改变合并为一条改变即添加了字符'ab')

对于多个文件(也就是当前git add过的所有文件)，上面叙述适用于每个文件

总结来说：此命令会显示对文件要commit哪些改变到分支上了
(show what's about to commit)

diff的输出是以行为标准的，若有以下diff输出：
```
- jello world
+ hello world
```
则表示这次改变效果相当于删除了jello行，新增了hello行
(无论你在文档里是怎样将j改为h的)

总结，git中对一行做修改被看作是删除此行再新增一行

```
$ git diff
```
对于单个文件：
查看该文件此时与最近一次add过后时的差异(换个说法就是查看该文件此时与在staging area中的快照的差异)

对于多个文件，上面叙述适用于每个文件<br>
官方说法为：<br>
Without --cached, git diff will show you any changes that you’ve made but not yet added to the index

没有--cached的话，git diff将为你显示所有你还未添加到索引对文件做出的改变

总结来说：此命令会显示对文件要add哪些改变到索引上了
(show what's about to add)

仓库repo(repository)

-m参数为摘要描述信息：
```
$ git commit -m 'my first commit msg'
```

# Viewing project history
At any point you can view the history of your changes using
```
$ git log
```

If you also want to see complete diffs at each step, use
```
$ git log -p
```

Often the overview of the change is useful to get a feel of each step
```
$ git log --stat --summary
```


如果`$ git log -p`输出很多内容还需要翻页，这时候按<kbd>esc</kbd>或者<kbd>ctrl+c</kbd>都不管用，只有按q才能退出来

# branch
A single Git repository can maintain multiple branches of development. To create a new branch named "test", use:
```
$ git branch test
```

If you now run:
```
$ git branch
```

you’ll get a list of all existing branches:
```
  test
* master
```
The "test" branch is the one you just created, and the "master" branch is a default branch that was created for you automatically. The asterisk marks the branch you are currently on; type

run:
```
$ git switch test
```
to switch to the test branch. Now edit a file, commit the change, and switch back to the master branch:
```
(edit file)
$ git commit -a
$ git switch master
```
Check that the change you made is no longer visible, since it was made on the test branch and you’re back on the master branch.

You can make a different change on the master branch:
```
(edit file)
$ git commit -a
```

at this point the two branches have diverged, with different changes made in each. To merge the changes made in test into master, run:
```
$ git merge test
```
If the changes don’t conflict, you’re done. If there are conflicts, markers will be left in the problematic files showing the conflict;

```
$ git diff
```
will show this. Once you’ve edited the files to resolve the conflicts,

```
$ git commit -a
```
will commit the result of the merge. Finally,
```
$ gitk
```
will show a nice graphical representation of the resulting history.

At this point you could delete the test branch with
```
$ git branch -d test
```
This command ensures that the changes in the test branch are already in the current branch.

If you develop on a branch crazy-idea, then regret it, you can always delete the branch with
```
$ git branch -D crazy-idea
```
Branches are cheap and easy, so this is a good way to try something out.

如果合并merge没有冲突那么用`git diff --cached`和`git diff`是没用的<br>
只有有冲突的时候才会显示差异

从主分支上从每个节点分出一个分支test，如果这时只改变master或者test其中一个分支当中修改文件提交，那么无论怎么修改两分支都不会存在冲突，只有当两个文件都有所修改才会存在冲突。这也符合冲突的本意。

gitk需要执行`$ sudo apt install gitk`安装

--------

A merge B,那么A就改变了，B并未改变
冲突过后，git会在文件中输入一些符号对修改进行标注，这些标注不一定非要删，即使你原封保留再git commit都没有问题，它们是git的标注符号但你commit的时候可以保留他们

--------

分支，为什么需要分支。设想你有一个项目，你已经开发到一个阶段，此时你commit保存了此时项目的状态(/快照)，你想从项目此时的快照添加a函数、aa功能看看什么效果，你也想从此时的快照出发添加b函数、bb功能看看什么效果，此时git中分支的功能就能派上用场了

快照为容器术语，对一个容器中或服务器中的一个操作系统保存一个快照即为对当时的系统做一个备份，如果后面系统出问题了等情况，就可以回溯到这些保存的快照时的状态了

执行：
```
$ git merge test
```
如果当前分支的文件内容完全包含了test分支文件的内容（test分支文件每一个字符在master分支文件上对应位置处都是相同的），则当前分支多于test的内容不会更新到test分支，控制台会提示：
```
"Already up to date."
```

这时只有切换到test分支上
```
$ git switch test
```
然后执行
```
$ git merge master
```
才会将master分支多余的部分更新到test分支上<br>
test合并master和master合并test都是没问题的，不存在master比较特殊是默认分支不允许被合并的情况


如果merge有冲突的话，你打开合并后的冲突文件发现你并不想合并了，<br>
这时可以执行
```
$ git merge --abort
```
就可以终止合并了，文件内容就为合并前的内容了。<br>
In case you've made a mistake while resolving a conflict and realize this only after completing the merge, you can still easily undo it: just roll back to the commit before the merge happened with "git reset --hard " and start over again.


如果在一分支上对一文件hello.md做了修改没有commit，这时切换到另一分支，shell会出现提示:
```
"M	hello.md"
switched to branch 'test'
```
代表文件hello.md被Modified了<br>
这时即使处在其它分支同样能看到对文件的修改，<br>
此文件在哪个分支commit的，此次对文件的修改就算在哪个分支的<br>
也就是说如果文件在master上add了，在test分支上同样能够commit此次add的修改。即对一个文件在一分支上add了，并不代表此次add的修改属于这个分支。也就意味着索引是公有的，不是每一个分支都单独拥有一个索引。只有commit过后，此次对文件的修改才属于commit时所在分支的。


# Git for collaboration
```
jw$ git clone /home/jw_git/alice myrepo
(edit files)
jw$ git commit -a -m 'msg'
```
```
alice$ cd /home/jw_git/alice
alice$ git pull /home/jw_git/jw/myrepo master
```
This merges the changes from Bob’s "master" branch into Alice’s current branch. If Alice has made her own changes in the meantime, then she may need to manually fix any conflicts.<br>
The "pull" command thus performs two operations: it fetches changes from a remote branch, then merges them into the current branch.
```
alice$ git fetch /home/jw_git/jw/myrepo master
alice$ git log -p HEAD..FETCH_HEAD
```

# 连接GitHub

连接GitHub之前请先执行下面配置
```
$ git config --global user.name "jack518"
$ git config --global user.email "baofengyujue@live.com"
```
执行下面命令，查看刚才的配置
```
$ git config --global --list
```

```
$ ssh-keygen -t rsa -C "baofengyujue@live.com"
```

```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/jack518/.ssh/id_rsa): id_rsa
```
这里id_rsa即为文件名，没有此文件会自动新建名为id_rsa的(公/私)钥文件
命令行输入字符串使用单引号或双引号都可以

（在Windows中，在哪个目录下打开git bash执行的上述命令，公私钥文件就生成在哪个文件。但是在Ubuntu下，生成的公私钥文件都保存在`~/.ssh/`目录下）

（在Windows中，如果在一个目录中生成了公私钥文件，可将私钥文件重命名为id_rsa保存到`~/.ssh/`目录中，然后将公钥内容复制保存到GitHub账号ssh列表里，这样在任一目录打开git bash执行相关连接命令就能与GitHub建立连接了）

（在Windows中，打开git bash，里面的~代表`/c/Users/JackWang/`）

---

注意这里最好命名为id_rsa,在windows上如果不命名为id_rsa,那么需要先使用命令`$ eval $(ssh-agent -s)`将ssh-agent进程打开，然后使用命令`ssh-add ~/path/to/my_key`将私钥文件my_key添加到ssh-agent

（如果不打开ssh-agent进程执行命令会出现`"Could not open a connection to your authentication agent."`的错误提示）

（使用ssh-add方式添加私钥过后在Windows当中重新打开git bash需要重新启动ssh-agent才行，可以参照官方说明，在`~/.profile` 或 `~/.bashrc`文件中添加一段shell代码，使打开终端时默认执行ssh-agent。注意，在Ubuntu中即使私钥名为自定义名，重新打开terminal与GitHub服务器连接也不需要再次添加私钥文件,应该是Ubuntu下默认公私钥文件都存放在`~/.ssh/`目录下，git在Linux系统中也默认记录了私钥文件名，所以才不需要再次添加了）
输入上述ssh-add命令回车后，会提示输入密钥passphrase，这时输入自己之前为该key设置的密码即可

---

关于公私钥文件：<br>
刚刚用ssh-keygen的方式创建出的公私钥是唯一配对的，所以当你将公钥复制粘贴到GitHub的ssh设置页并保存后，本地与GitHub服务器连接时，git会用相应算法如rsa比较本地的私钥得出确实是匹配的过后才会允许连接。

If your private key is not stored in one of the default locations (like ~/.ssh/id_rsa), you'll need to tell your SSH authentication agent where to find it. To add your key to ssh-agent, type ssh-add ~/path/to/my_key
[参见官方说明文档][1]

如果即不将私钥文件保存为`~/.ssh/id_rsa`，又不通过ssh-add添加私钥文件到ssh-agent，那么就有可能用`$ ssh -T git@github.com`测试连接时出现"git@github.com: Permission denied (publickey)."的错误提示。当然肯定也没有与GitHub建立上连接。

需要注意的是在ubuntu中，即使私钥名不为id_rsa，也不通过ssh-add添加私钥文件，同样可以与GitHub建立连接，但尽量不要这么做，如果碰到问题请参考上述说明。

对于：
```
Enter passphrase (empty for no passphrase):
```
这里提示输入一个密码，我输入的是zhang<br>
这里密钥可以只为一个字符,比如'a'<br>
注意，此密码为私钥的密码。<br>
注意，设置密钥过后，vscode需要在git bash中打开,以此来继承git bash中的ssh环境，这样才能够使用vscode自带的一些远程连接操作比如push/pull，而且后续与远程连接都需要输入私钥密码。<br>
不设置密码的话也不需要这些额外的操作了，看自己实际需求。

对于：
```
Your identification has been saved in id_rsa
Your public key has been saved in id_rsa.pub
```
这两个文件保存在`~/.ssh/`目录内的

```
$ cat id_rsa.pub
```
会将id_rsa.pub的内容输出到shell，在ubuntu终端中鼠标选择这段文本，<kbd>ctrl+shift+c</kbd>复制，<br>
打开GitHub网站，在设置中找到`SSH and GPG keys`一栏，在右边相应文本框中粘贴添加该公钥
(在Windows中，右键用记事本打开id_rsa.pub公钥文件，然后全选复制就行了)

对于：
```
$ ssh -T git@github.com
```
用这条命令测试是否能与GitHub服务器建立连接，执行这段命令后会提示：
```
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
```
输入yes，然后后面会出现一个弹框提示输入密码，这时输入我们私钥的密码zhang就可以了(注意不是系统的登陆密码)，最后一句说的是在`~/.ssh/`目录下多了一个known_hosts文件，这个文件就是用来记录刚刚连接的host的。

```
Hi baofengyujue! You've successfully authenticated, but GitHub does not provide shell access.
```
如果有上面提示，说明与GitHub连接成功

在GitHub网站上创建一个repo，并将这个repo的ssh复制下来<br>
然后执行：
```
$ git remote add origin git@github.com:baofengyujue/git_p1.git
```
按照惯例，远程名字(remote_name)一般都取做origin

在本地做出一些修改并提交到本地repo后，执行以下命令
```
$ git push -u origin master
```
将本地的repo上传push到GitHub服务器

Note the -u (also known as set upstream) flag only needs to be used on the initial push, all subsequent pushes can leave it out.<br>
如果远程repo刚创建只有一个主分支，那么'-u'也是可以省略的（待研究）

如果第一次没有commit就git push -u的话，会出现下面报错：
```
error: src refspec master does not match any
error: failed to push some refs to 'https://github.com/baofengyujue/learning_powershell.git'
```


`git checkout test`与`git switch test`都为切换到test分支

Pro Tip: We can use the command git checkout -b [branch name] to both create and change to the branch name specified.

对于：
```
$ git remote add origin https://github.com/baofengyujue/p2.git
```
通过https协议连接
```
$ git push -u origin master
```
这时会提示输入GitHub用户名和密码：
```
Username for 'https://github.com': baofengyujue         
Password for 'https://baofengyujue@github.com':
```

如果远程是添加的https协议，那么在vscode中与远程连接时会打开浏览器提示将GitHub账号授权给vscode，点击授权，然后此授权信息就传回了vscode，这时vscode会有一个提示框信息"Allow an extension to open this uri"，点击open就可以了。

授权过后，即使是与GitHub上其它repo建立本地repo连接都不再需要打开浏览器授权了，vscode在本地已经记住你的GitHub账户信息了

如果在一个分支中新建了一个文件并add了，那么在切换分支时会有提示：
```
A	intro.md
Switched to branch 'master'
```
表示intro.md为append新建的，应该是这个意思。
```
$ git remote -v
```
which shows you the URLs that Git has stored for the shortname to be used
when reading and writing to that remote
```
$ git fetch <remote_name>
```
从远方仓库拿取本地仓库还没有的数据，下载的数据只是存在了本地repo中，并没有更改本地repo中的其它任何项目文件。

If you clone a repository, the command automatically adds that remote repository under the name “origin”. So, `git fetch origin` fetches any new work that has been pushed to that server since you cloned (or last fetched from) it.

```
$ git pull
```
相当于执行了git fetch和git merge。后面不用跟参数就行，分支什么的都是使用默认的。<br>
If your current branch is set up to track a remote branch, you can use the `git pull` command to automatically fetch and then merge that remote branch into your current branch.

```
$ git clone git@github.com:baofengyujue/git_p1.git
```
将git_p1仓库克隆到当前目录，并将本地主分支追踪到远程主分支。<br>
the `git clone` command automatically sets up your local master branch to track
the remote master branch (or whatever the default branch is called) <br>
注意GitHub上如果给新建的repo有些主分支默认叫main，不叫master

```
$ git remote
```
显示本地添加的remote的名称

```
$ git remote -v
```
which shows you the URLs that Git has stored for the shortname to be used
when reading and writing to that remote:
```
origin	git@github.com:baofengyujue/git_p1.git (fetch)
origin	git@github.com:baofengyujue/git_p1.git (push)
```

If you want to see more information about a particular remote, you can use the `git remote show <remote>` command. 例如：
```
$ git remote show origin
```


Can I use SSH Git authentication with VS Code?#
Yes, though VS Code works most easily with SSH keys without a passphrase. If you have an SSH key with a passphrase, you'll need to launch VS Code from a Git Bash prompt to inherit its SSH environment.
引用自(https://code.visualstudio.com/docs/editor/versioncontrol)

对于：
```
$ eval $(ssh-agent -s)
```
该命令可后台打开ssh-agent,必须得先开启才能进行下面操作，默认是没有开启的
```
$ ssh-add ~/path/to/my_key
```
上面命令可添加密钥到ssh-agent
```
$ ssh-add -l
```
上面命令可查看ssh-agent里有多少个添加的key。<br>
ssh-agent进程只属于每个git bash窗口的，在一个窗口中打开ssh-agent，在其它git bash窗口中却没有ssh-agent进程。如果关闭窗口，进程也会自动结束。

可以通过执行`$ ssh-add`命令（不带参数）判断ssh-agent是否开启，如果返回`Could not open a connection to your authentication agent.`那么就是没有开启，如果开启了此命令会默认添加~/.ssh/id_rsa私钥文件，如果私钥文件有密码会提示输入密码。


如果是建立ssh连接的话，在外面打开vscode，从source control面板点击'...'更多选项里面的push的话，如果私钥有passphrase那么就会弹出错误提示窗口，提示permission denied(public key),需要在git bash中使用命令`code <文件名>`，然后再push，这时会弹出一个OpenSSH提示框,提示输入passphrase，输入私钥的密码即可正常push了。如果在git bash中没有ssh-add私钥文件的话，那么无论是在git bash中，还是在vscode中每次与远程连接都会提示输入私钥密码。ssh-add私钥文件了的话，从git bash中打开vscode，或者在git bash中push的时候都不再需要私钥密码。

特别注意！：要想vscode继承git bash中当前的ssh环境，必须先完全退出vscode，否则命令行打开文件只会在现有vscode窗口中打开一个文件选项卡窗口，根本没有继承到ssh环境。

想要每次启动git bash自动开启ssh-agent可以参考[官方文档][1]中"Auto-launching ssh-agent on Git for Windows"给出的shell代码片段。

另外，在Ubuntu中即使不从终端打开vscode，vscode也能正常与远程进行ssh连接（之前应该要在命令行输入过私钥密码让系统记住了此密码或者说此权限），根本都不需要启动ssh-agent,而且vscode与远程连接时也不需要输入私钥密码（应该是系统的凭证管理系统记住了此私钥密码吧，或者说是git，不知道）

总之，在Ubuntu上建立ssh连接是很方便的，公私钥文件不需要命名为id_rsa,也不需要处理ssh-agent进程（当然也可以操作此进程），私钥密码或者说私钥权限也会被记住，不用每次输入密码，如果之前输入过私钥密码，那么vscode中与远程进行ssh连接完全就不需要额外的操作，都不需要输入密码。




在vscode中，同步按钮执行的是pull&push，所以如果本地与远程有冲突，那么在pull的时候就会显示出冲突了。


vscode的source control面板中，其将git的add命令称为stage，注意后文说法

在vscode中（或者对于git来说），重命名一个文件然后stage的话会，git会将重命名的文件当作一个新文件对待，然后之前名字的文件会在vscode的source control面板当中显示为deleted，显示为灰色的老文件名被划掉的样子，注意，此时要想远程同步也删除此文件的话，也要stage此文件。老文件被删除了，老文件的一些commit记录自然已不在，而且新文件也不会继承老文件的commit记录，因为是新文件嘛，所以有需要保留commit记录的情况下的话，首先要把文件名想好，实在要改而且还要保留之前commit记录的话，可以别把老文件stage了。


vscode的source control面板中的缩写字母含义:

- D---deleted
- M---modified
- U---untracked
- A---added
- C---conflict



vscode的source control面板中的repo名为当前repo的目录名（或者叫做文件夹名）
所以如果在网页重命名了repo，那么想在source control面板中也显示为新名字，要么把原本地repo删了重新clone到本地用vscode打开，要么就重命名repo的目录名再重新用vscode打开目录。

对于：
```
JackWang@DESKTOP-BH3PFON MINGW64 /d/xampp/htdocs/jw_git/my_notes (main)
```
最后的"(main)"表示当前在名为main的分支上，这是GitHub重命名的主分支名

GitHub是通过`git branch -M "main"`这一句重命名主分支为main的


## 直接通过git clone连接GitHub

在GitHub网站创建repo<br>
然后执行：
```
$ git clone https://github.com/baofengyujue/saved_notes.git
```
这时命令行会输出：
```
Cloning into 'saved_notes'...
warning: You appear to have cloned an empty repository.
```
这时就已经和远程建立了连接了，<br>
也不需要git add remote或者-u push了，也不需要在本地创建repo了

如果目录里已经有了要git clone下来的repo名的目录了，那么并不会clone远程repo，而是报错：
```
fatal: destination path 'learning_excel' already exists and is not an empty directory.
```



## 总结GitHub连接方式(https)

myrepo/目录中存放的项目文件<br>
两种方式都已经提前在GitHub网站将myrepo仓库创建好了<br>
<myrepo_remote>即为仓库远程https地址

git remote add方式：
```
cd myrepo/
git init
git add .
git commit -m 'init'
git remote add origin <myrepo_remote>
git push -u origin master
```

git clone方式：
```
git clone <myrepo_remote>
cd my myrepo/
(复制粘贴项目文件到myrepo/)
git add .
git commit -m 'init'
git push origin master
```

部分内容引用自：

- https://git-scm.com/docs/gittutorial

- 《Pro Git book》：
https://git-scm.com/book/en/v2



[1]:https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/working-with-ssh-key-passphrases



```
nano编辑器：
其中^代表ctrl，M代表Alt，(meta key)
carriage return--CR--\r
line feed--LF--\n
DOS \r\n
MAC \r
Unix \n

对于nano编辑器write out表示存文件，read file表示打开文件，nano编辑器还会将保存文件称作save buffer，如果文件没有保存exit的话会提示保存，如果想将当前编辑器的文件追加到原文件的话就append，但很少有人会打开一个文件，然后又将同样的内容追加到文件末尾，基本都是覆盖原文件。如果想编辑器的内容存到目录中的一个新文件上，可以选择to files,然后上下左右选到新文件名enter覆盖保存。如果还没保存文件，查看目录会看到目录中会有一个同名的.swp文件，这应该就是我们正在编辑器中编辑的文本缓存文件了.
```