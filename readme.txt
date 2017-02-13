git init dir 初始化仓库
git add filename  将文件添加到仓库(暂存区)
git commit -m 'comment' 将文件提交到仓库(当前的分枝)
git status 查看状态
git log 查看历史
git log --pretty=oneline 查看历史 查看一行 一般用于回到历史版本
git reset --hard HEAD^ 回退到之前的版本
git reset --hard 3628164 回到未来的版本
git reflog 查看每一次命令  包括每一次版本的id，一般用于回到未来的版本

git diff HEAD -- readme.txt 查看工作区和版本库里面最新版本的区别
git checkout -- readme.txt
      一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
      一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

git reset HEAD readme.txt
      git reset命令既可以回退版本，
      也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

小结
  场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

  场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

  场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

版本库删除文件：
  直接在文件系统中删除
        1. 确定要删除   git rm filename
        2. 误删         git checkout -- filename

生成秘钥：
    ssh-keygen -t rsa -C "youremail@example.com"
    文件在用户主目录下的.shh/id_rsa.pub
    将用户的秘钥添加进远程仓库管理的秘钥中，该用户就可以向仓库中添加修改文件

将本地仓库与远程的仓库同步
    1.本地仓库与github仓库连接
    git remote add origin git@github.com:yaojinzhuzhu(用户名)/learngit(用户名).git
    2. 重新设置连接
    git remote set-url origin git@github.com:username/repo_name.git
    3.本地库的所有内容推送到远程库上(第一次)
    git push -u origin master
    4.本地库的所有内容推送到远程库上(第n次)
    git push origin master(如果出现错误则使用 git pull origin master)

克隆远程仓库
   clone git@github.com:jinyaozhuzhu(用户名)/learn(用户名).git
