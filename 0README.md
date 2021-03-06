工作区 -> git add -> 暂存区 -> git commit -> 版本库 -> git push -> 远程仓库

版本库HEAD表示当前版本，上一个版本HEAD^，上上一个版本HEAD^^.......

配置本机git
git config --global user.name "abcd"
git config --global user.email abcd@efgh.com

git连接自己的github
第一步：创建SSH_key: ssh-keygen -t rsa -C "aaadega@gmail.com",
        创建成功后.ssh文件下的id_rsa是私钥，id_rsa.pub是公钥
第二步：登陆GitHub，打开“Account settings”，“SSH Keys”页面
第三步：添加SSH key，用公钥

一切从版本库同步到远程的git都有uname和pword操作push
其他都是从本地直接切换操作

一定要从自己的账号下clone仓库，这样你才能推送修改。先fork再clone

git init 初始化此文件夹为版本库
git add readme.txt 将文件从工作区添加到暂存区
git checkout -- readme.txt 撤销工作区文件的修改
git commit -m "this is commit" 将文件从暂存区提交到版本库
git reset HEAD 从暂存区回退到工作区
git diff 查看工作区文件修改前后的差异
git status 查看工作区与暂存区的文件修改状态
git log 查看提交日志
git log --pretty=oneline
git reset --hard HEAD^ 回退到上一版本
git reflog    查看命令历史
git remote add origin url 为远程Git更名为origin
git push -u origin master 首次推送此次修改
git push origin master 然后可以不加-u
git clone url 克隆一个远程库到本地
git branch page 创建新分支
git checkout page 选择新分支
git checkout -b page 相当于上面两条一起
git branch 查看分支
git merge page 合并分支page到master，checkout到master分支
git merge page --no-ff -m "plain" 禁用Fast forward
git branch -d[D] page 删除分支page，删除前先切换到master分支[D强行删除]
git push origin :page  删除远程分支page
cat read.txt   查看文件内容（冲突）
git log --graph 查看分支合并情况
git log --graph --pretty=oneline --abbrev-commit  查看最近分支合并情况
git stash 隐藏当前工作区
git stath list 查看隐藏的工作区
git stash apply stash{0} 恢复隐藏的工作区，不会删除stash
git stash drop 删除stash
git stash pop stash{0} 恢复隐藏的工作区，一并删除stash
git remote -v 查看远程库信息
git pull 拉取最新分支
git branch --set-upstream branchName origin/branchName 指定本地与远程之间的链接
git tag <name>  可以打一个新标签
git tag   查看所有标签
git tag name id  打标签
-a指定标签名，-m指定说明文字
git show <tagname>可以看到说明文字

git 下载其他branch并同步本地branch

有时git clone下来会出现很多branch，更麻烦的是如果主分支没代码那你就只能看到.git目录了。如下面的这个:

$ git clonegit://gitorious.org/android-eeepc/mesa.git


　　发现本地就只有一个.git目录，那么这个时候就需要checkout了。

　　进入你的本地目录，如这个是mesa，利用

$ git branch –r

　　查看branch信息（当然你也可以用git show-branch查看，不过有时并不好用），获得如下branch信息：

origin/android
origin/mesa-es
origin/mesa-es-dri

　　此时我们需要的是android分支的代码，那么此时就要进行checkout了。
$ git checkout origin/android

　　你再看你的目录（mesa）下是不是有了代码了？其它的branch同理。
　　
　　git clone默认会把远程仓库整个给clone下来; T2 {0 t, l+ @0 U" C2 g) i
但只会在本地默认创建一个master分支
如果远程还有其他的分支，此时用git branch -a查看所有分支：

* master   
remotes/origin/HEAD -> origin/master   " A4 u3 ~+ n5 u5 \7 R" Z( d# J
remotes/origin/master   
remotes/origin/python_mail.skin   
remotes/origin/udisk   
remotes/origin/vip
复制代码
能看到远程的所有的分支，如remotes/origin/python_mail.skin  e  Y' X9 ~, f1 |
可以使用checkout命令来把远程分支取到本地，并自动建立tracking

$ git checkout -b python_mail.skin origin/python_mail.skin) X& X: I3 Q; ?9 j9 T5 @; J/ M
Branch python_mail.skin set up to track remote branch python_mail.skin from origin.; i/ B! ^3 J# u6 a( }. I$ M- i
Switched to a new branch 'python_mail.skin'
复制代码
或者使用-t参数，它默认会在本地建立一个和远程分支名字一样的分支
折叠展开复制代码

$ git checkout -t origin/python_mail.skin
复制代码
也可以使用fetch来做：

$ git fetch origin python_mail.skin:python_mail.skin
复制代码
不过通过fetch命令来建立的本地分支不是一个track branch，而且成功后不会自动切换到该分支上- z) t: R4 p- s6 _2 d3 a
注意：不要在本地采用如下方法：

$ git branch python_mail.skin
$ git checkout python_mail.skin/ i8 z/ N: a% v/ Q: M
$ git pull origin python_mail.skin:python_mail.skin
复制代码
因为，这样建立的branch是以master为基础建立的，再pull下来的话，会和master的内容进行合并，有可能会发生冲突... 
　　
　　
　　
　　
GIT FETCH VS GIT PULL

1. git fetch：相当于是从远程获取最新版本到本地，不会自动merge
    
git fetch origin master
git log -p master..origin/master
git merge origin/master

    以上命令的含义：
   首先从远程的origin的master主分支下载最新的版本到origin/master分支上
   然后比较本地的master分支和origin/master分支的差别
   最后进行合并
   上述过程其实可以用以下更清晰的方式来进行：
 git fetch origin master:tmp
git diff tmp 
git merge tmp

    从远程获取最新的版本到本地的test分支上
   之后再进行比较合并
2. git pull：相当于是从远程获取最新版本并merge到本地
 git pull origin master

上述命令其实相当于git fetch 和 git merge
在实际使用中，git fetch更安全一些
因为在merge前，我们可以查看更新情况，然后再决定是否合并
