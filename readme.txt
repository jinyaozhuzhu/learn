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
      一种是readme.txt自修改后还没有被放到暂存区，现在，
              撤销修改就回到和版本库一模一样的状态；
      一种是readme.txt已经添加到暂存区后，又作了修改，
              现在，撤销修改就回到添加到暂存区后的状态。

git reset HEAD readme.txt
      git reset命令既可以回退版本，
      也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

小结
  场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

  场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，
        第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

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

分支管理：
    创建与切换：git checkout -b dev
      相当于：git branch dev 创建
            git checkout dev 切换

    查看：git branch (当前分支在名字前加*)
    合并: git merge branchName
    删除：git branch -d branchName

    查看分支合并图：git log --graph

分支管理策略
    通常：Git会用Fast forward模式，删除分支后，会丢掉分支信息。
    禁用Fast forward模式：Git就会在merge时生成一个新的commit，
          这样，从分支历史上就可以看出分支信息。
    命令：  git merge --no-ff -m "merge with no-ff" dev

Bug 分支管理
    流程：1. 储藏当前的工作现场。命令，git stash
          2. 切换到出现bug的分支。命令，git checkout bugBranch
          3. 在bug分支创建临时分支。命令，git chechout -b tempBranch
          4. 在tempBranch上修复bug
          5. 在bugBranch上合并且删除tempBranch
          6. 接着切换开发分支上.命令，git checkout devBranch
          7. 查看开发分支上的工作现场。命令，git stash list
          8. 恢复工作现场。
            8.1 先恢复，后删除stash内容。命令，git stash apply
                                            git stash pop
            8.2 恢复删除一起操作，git stash pop
          9. 恢复到指定的现场，
              git stash list
              git stash apply stash@{0}(工作线程的代号)

强行删除一个未合并的分支：
      git branch -D branchName

多人协作：
    查看远程库信息：git remote
    详细信息：git remote -v

    推送分支：git push origin branchName
    一般把主分支(master)以及开发分支(dev)与远程同步

    抓取分支(clone)：
        1. 通常只能看到master分支。命令同上。
        2. 开始时创建远程的dev分支，git checkout -b dev origin/dev

    多人开发冲突：
        1.git pull 把最新的提交从origin/dev抓下来
        2. 第一步若失败，则指定本地dev分支与远程origin/dev分支的链接
        3. git pull 把最新的提交从origin/dev抓下来

标签管理：
    创建标签：
        1. 切换要打标签的分支。命令 git branch branchName
        2. 打标签。命令 git tag tagName
        3. 查看当前分支的标签。 git tag
        4. 给对应提交版本打标签。 git tag v_num commit_id
        5. 查看版本信息。git show v_num
        6. 创建带有说明的标签，用-a指定标签名，-m指定说明文字
            git tag -a v0.1 -m "version 0.1 released" 3628164
        7. 通过-s用私钥签名一个标签
            git tag -s v0.2 -m "signed version 0.2 released" fec145a
        8. 删除标签。命令，git tag -d v_num
        9. 推送某个标签到远程。命令，git push origin v_num
        10 一次性推送全部尚未推送到远程的本地标签：
            git push origin --tags
        11. 删除推送到远程的标签
            11.1 本地删除。  git tag -d v_num
            11.2 远程删除。  git push origin :ref/tags/v1.0

自定义Git：
     忽略特殊的文件。
        Git工作区的根目录下创建一个特殊的.gitignore文件，
            然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
      配置别名：
        例如，git config --global(表示对当前的用户有作用) alias.st status
        可以直接使用 git st 替代 git status
        git config --global alias.lg "log --color --graph --pretty=
          format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)
          %C(bold blue)<%an>%Creset' --abbrev-commit"

      删除别名：
        1. 进入 .git/config， 直接删除对应的行
        2. 进入 .gitconfig 
