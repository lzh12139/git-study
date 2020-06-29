# git常用操作


## 开始

设置提交时显示的用户名，邮箱：
```
git config --global user.name "xxx"
git config --global user.email "xxx@xxx.com"
```
网络配置：
```
git config --global http.https://git.code.oa.com.proxy http://127.0.0.1:12639
git config --global http.http://git.code.oa.com.proxy http://127.0.0.1:12639
```
如果要取消这个设置，那么：
```
git config --global --unset http.https://git.code.oa.com.proxy
git config --global --unset http.http://git.code.oa.com.proxy
```

本文中所有 `<>` 括起来的内容为必选项， `[]` 括起来的内容为可选项。  
`SHA` 指 commit 的 hash 值。


## 一些概念

1. 工作区  
   在电脑里能看到的目录，供人们操作文件的地方。

2. 暂存区  
   临时存放你所作出的改动，事实上它只是一个文件，保存即将提交的文件列表信息。

3. 版本库  
   存放实际数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本。

他们之间的关系和操作可以看下图：

![avatar](/image/git-file-change.jpg)


## 基础操作

1. 创建仓库  
    - 通过 `init` 初始化仓库  
        ```
        git init                        初始化本地仓库
        git remote set-url origin <url> 与远程库相关联
        ```

    - 通过 `clone` 初始化仓库  
        ``` 
        git clone <url> [dir] 将地址为 url 的库，放在 dir 目录下。
        ```

2. 为下次 `commit` 添加文件
   ```
   git add <file>
   ```
   将文件添加到暂存区。

3. 提交更新
    将暂存区中的文件提交到版本库中，可以一次提交多个文件
    ```
    git commit -m "<message>"
    ```

    将已追踪所有的文件暂存，通常搭配 -m 一起用
    ```
    git commit -a 
    ```

    更改最后一次提交的message  
    **注意，请不要修改已经推送过的提交记录**
    ```
    git commit --amend -m "<message>"
    ```

4. 推送
    ```
    git push <远程主机名> <本地分支名>:<远程分支名>
    ```
    将本地分支的更新，推送到远程主机。
    
    可以使用 `-u` 选项指定上游分支，这样以后就可以不加任何参数地使用 `git push` 了。  
    如果不指定远程分支，则默认将本地的该分支推送到远程的相同名字的分支上。  
    如果远程仓库不存在该分支，则该分支会被自动创建

5. pull  
    将远程仓库的最新更改合并到当前分支中，相当于 `fetch + merge`
    ```
    git pull [主机名] [分支名]
    ```
    不推荐这样做，建议 `fetch + merge` ，
    或者使用
    ```
    git pull --rebase [主机名] [分支名]
    ```

6. fetch  
    用于取回远程仓库所有分支的最新快照，并保存在版本库中。

    创建并更新所有远程分支的本地远程分支，`FETCH_HEAD` 是字典序最小的那个分支？（存疑，但还未找到对应的说明）
    ```
    git fetch [主机名]  
    ```

    可以使用
    ```
    git fetch --all
    ```
    来拉取所有远程仓库的所有分支
    
    仅拉取对应的分支，`FETCH_HEAD` 等于这个分支
    ```
    git fetch <远程主机名> <分支名> 
    ```
    拉取之后，可以用 `<主机名>/<分支名>` 来代表最新的分支。

7. status  
    显示工作目录和暂存区的状态。  
    从上到下依次显示 已暂存，已修改，未追踪 的文件。  
    编写 `.gitignore` 文件（见第 8 点）可以忽略不想让 git 追踪的文件。

8.  .gitignore  
    在仓库根目录下的配置文件，用于设置忽略某些文件，让他们不被 git 所跟踪。  
    具体编写规则可以参考 https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93

9.  diff  
    ```
    git diff                        比较工作区和暂存区的差异。
    git diff <file>                 比较这个文件的工作区与暂存区的差异。
    git diff <branch1> <branch2>    比较两个分支的差异。
    git diff <SHA1> <SHA2>          比较两次提交的差异。
    ```

    常用选项
    ```
    --staged                        比较暂存区和版本库的差异。
    ```

10. log
    ```
    git log                 查看当前分支的提交详情
    git log <SHA>           查询该提交之前的记录，包含该提交
    git log <SHA1> <SHA2>   查询 提交1 与 提交2 之间的记录，包括提交1 与 提交2 
    git log <SHA1>..<SHA2>  查询 提交1 与 提交2 之间的记录，只包括提交1
    ``` 

    常用选项
    ```
    --oneline           在一行中显示
    --all               查看所有分支的记录
    -p                  按补丁格式显示每个提交引入的差异。  
    --graph             在日志旁以 ASCII 图形显示分支与合并历史。 
    --follow <file>     该文件的改动，包括重命名
    -S "<代码>"         按代码追踪，支持正则表达式
    ```

11. stash  
    当想切换分支，但是又不想提交文件时，可以使用暂存，将新的贮藏推送到贮藏栈上。  
    会分别对暂存区和工作区的状态进行保存。
    添加 `-k` 选项，可以保存暂存区状态  
    添加 `-p` 选项，会交互式地询问哪些改动需要贮藏
    ```
    git stash                       暂存文件
    git stash list                  查看暂存列表
    git stash push -m <message>     
    ```
    
    取出贮藏，将暂存应用到工作区可以使用以下的命令：  
    如果不指定取出的贮藏，则默认取出栈顶，也就是最新的那个。  
    如果不添加 `--index` 选项，那么将所有该贮藏的改动恢复到工作区。
    ```
    git stash pop [stash@{n}]       取出贮藏并删除该贮藏
    git stash apply [stash@{n}]     取出贮藏，但不删除该贮藏
    ```

    删除贮藏
    ```
    git stash drop [stash@{n}]      删除单个贮藏
    git stash clean                 删除所有贮藏
    ```

    在某一贮藏的基础上建立分支
    ```
    git stash branch <分支名>
    ```

12. clean
    清除一些不想要的文件
    ```
    git clean -dn       查看会清除什么文件
    git clean -fd       清除文件
    ```

12. rm 
    ```
    git rm <file>               同时删除磁盘上和暂存区上的文件
    git rm --cached <file>      仅删除暂存区上的文件
    ```  
    将文件从已跟踪文件清单中移除需要使用上面的命令，而不是直接在文件目录中删除该文件。这样会删除 `git` 仓库中和磁盘上的该文件。  
    注意，如果要将该文件从 `git` 仓库中删除的话，不能只是删除本地文件

13. remote
    查看配置的远程仓库服务器
    ```
    git remote          查看配置的远程仓库服务器，只显示主机名
    git remote -v       查看配置的远程仓库服务器，显示主机名和url
    ```

    ```
    git remote add <主机名> <url>       添加新的远程仓库服务器
    git remote set-url <主机名> <url>   更改某个远程仓库服务器的url
    ``` 
    查看配置的远程仓库服务器。  
    以 `git` 开头的使用 `SSH` 公钥认证  
    以 `https` 开头的使用 `HTTPS` 认证，需要输入密码

14. 密钥  
    可以使用 SSH公钥 来进行认证，将公钥传到 github 上后 `push` 操作就不需要输入用户名和密码。  

    注意，只有在使用 SSH协议 时，才会通过 SSH公钥 来认证。  
    如果不是使用 SSH协议 ，那么请看第13点来更改。

    请直接看：    
    http://8000.oa.com/v2/KnowledgeStore/Article/KB201902190014

15. 设置别名
    ```
    git config --global alias.unstage 'reset HEAD --'
                              ^^^^^^^  ^^^^^^^^^^^^^
                              别名     原名
    ```
    这会使下面的两个命令等价：
    ```
    git unstage fileA
    git reset HEAD -- fileA
    ```

16. 一些通用选项  
    - `-p` 显示详细信息
    - `-f` **强制**动作


## 合并

1. cherry-pick  
    用于合并某个提交到当前分支  
    首先需要查看想要合并的提交的 hash 值，并切换到被合并的分支  
    然后执行
    ```
    git cherry-pick <SHA>
    ```
    有冲突就解决冲突，解决完 `git add` 就可以了

    如果想要取消 `cherry-pick` ，执行：
    ```
    git cherry-pick --abort
    ```

2. merge  
    用于合并某个分支到当前分支
    ```
    git merge --no-ff <分支名>
    ``` 

    如果合并过程中出现了冲突，使用命令
    ```
    git status
    ```
    查看冲突的文件名，并解决冲突，不要忘记添加他们

    为了避免污染主分支的commit信息，推荐使用 `--no-ff` 选项  
    可以使用 `--no-commit` 选项避免自动进行提交


## 版本回滚

1. HEAD  
    记指向**本地的当前分支的当前版本**的指针为 `HEAD`   
    前一个版本为 `HEAD^` ，前两个版本为 `HEAD^^` ，  
    前100个版本为 `HEAD~100`  
    在执行 `reset` , `pull` , `merge` 操作之后，会把操作前的 `HEAD` 放入 `ORIG_HEAD` 中，以方便回滚操作

2. 撤销暂存
    ```
    git reset HEAD <file>
    ```

3. 回退版本
    ```
    git reset --hard <SHA | HEAD~n>
    ```
    强制放弃所有更改，并将整个工作区**回退到**指定版本

4. 回退commit
    ```
    git reset --soft <SHA | HEAD~n>
    ```
    撤销 `commit` ，但是不会撤销文件的更改。  
    可以用来保存当前分支的修改，然后切换到另一分支。  

    常用方法： 刚刚 `commit` 错了，执行
    ```
    git reset --soft HEAD^
    ```
    因为刚刚的commit错了，而HEAD指向最新的commit  
    所以要回退到HEAD的前一个版本，也就是HEAD^  
    更改的文件会保留在暂存区中

5. 撤销某个commit  
    撤销后会自动commit，不建议。
    ```
    git revert <SHA | HEAD~n>
    ```

    可以用过加上选项 `-n` ，使得该次操作不会自动提交，改动会保留在暂存中。
    ```
    git revert -n <SHA | HEAD ~n>
    ```

    这个操作相当于做一次所选择的 `commit` 的逆操作，  
    即如果原 `commit` 是插入，那么这次就是删除。

6. 放弃某个文件本地修改  
    放弃某一文件的本地修改。  
    先从尝试从暂存区恢复，找不到再从版本库恢复  
    回退到该文件最近一次 `add` 或 `commit`  
    ```
    git checkout -- <file>
    ```

7. 回退某个文件  
    将某个文件回退到指定版本
    ```
    git checkout <SHA | HEAD~n> <file>
    ```

8. 提交历史  
    ```
    git log     查看所有提交过的版本信息
    git reflog  查看所有操作记录，包括被回滚过的操作
    ```


## 分支

1. branch  
    ```
    git branch                              查看本地分支
    git branch -a                           查看所有分支，包括远程的
    git branch <分支名>                     新建分支
    git branch -d <分支名>                  删除分支
    git branch -m <原分支名> <新分支名>     更改分支名字
    ```

2. checkout  
    用于切换分支。
    ```
    git checkout <分支名>
    ```

    新建并切换分支，  
    如果指定了源分支名就在源分支的基础上建立新分支；  
    否则在当前分支的基础上建立新分支
    ```
    git checkout -b <分支名> [<源分支名>]   
    ```

3. 删除远程分支
    ```
    git push <主机名> --delete <分支名>
    ```


## rebase

0. rebase的缺点  
    `rebase` 实际上是抛弃一些已有的提交，转而新建一些提交来代替原有的提交。  
    **不应该对任何推送过给远程仓库的commit执行变基**  

1. 分支合并  
    假设现在有两个分支 master 和 experiment 各自提交了更新。  
    由于使用 `merge` 合并会让 commit 变得很乱，  
    于是我们使用 `rebase` 来合并他们。

    ![avatar](/image/rebase-1.png)

    首先，我们切换到 experiment 分支，并执行 `rebase` 操作：
    ```
    git checkout experiment
    git rebase master
    ```

    过程中可能会有冲突，打开冲突文件并处理好冲突（跟 `merge` 一样）  
    冲突解决完后，不要忘记执行 `git add` 添加修改后的程序  
    然后继续执行以下命令，直到 `rebase` 操作完成。  
    注意不需要执行 `git commit` ，只需要执行下面这个命令就可以了。
    ```
    git rebase --continue
    ```

    在过程中随时都可以放弃：
    ```
    git rebase --abort
    ```
    这样会终止 `rebase` 操作，并且分支会回到 `rebase` 开始时的状态

    操作完成之后，experiment 分支就是我们要的结果：

    ![avatar](/image/rebase-2.png)

2. 合并commit  
    要合并最近几次的commit，使用：
    ```
    git rebase -i HEAD~n
    ```
    其中 `n` 是要合并几个 commit

    执行命令后会进入一个编辑器，里面显示的内容大概像下面一样：
    ```
    pick cacc52da add: qrcode
    pick f072ef48 update: indexeddb hack
    pick 4e84901a feat: add indexedDB floder
    pick 8f33126c feat: add test2.js

    # Rebase 5f2452b2..8f33126c onto 5f2452b2 (4 commands)
    #
    # Commands:
    # p, pick = use commit
    # r, reword = use commit, but edit the commit message
    # e, edit = use commit, but stop for amending
    # s, squash = use commit, but meld into previous commit
    # f, fixup = like "squash", but discard this commit's log message
    # x, exec = run command (the rest of the line) using shell
    # d, drop = remove commit
    ```
    需要更改的是前面几行的第一个单词  
    `pick` 表示使用该 `commit`  
    `squash` 表示使用该 `commit` ，把这个 `commit` 压缩到前面标记为的 `pick` 的 `commit` 中（也就是说，`commit` 的数量减少1）

    如果要将所有 `commit` 压缩成一个，可以保留第一个 `commit` 的 `pick` ，将其他所有 `commit` 改成 `s` (或 `squash` )  
    保存后，会弹出用于编写 `commit` 的编辑器，更改 `commit` 并保存后，就完成了 `commit` 的压缩

3. 可以看看这个，讲的很不错  
    https://mp.weixin.qq.com/s/Y-imESQiYRCZXJd_mkkoZw


## 子模块

1. 定义  
    子模块允许你将一个 `Git` 仓库作为另一个 `Git` 仓库的子目录。   
    它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

2. 添加子模块  
    增加一个子模块，可指定分支，并将子模块放在设置的路径里。  
    如果不指定路径，则默认放在与项目名同名的文件夹中。
    ```
    git submodule add <url> [-b branch] [path]
    ```

3. 克隆包含子模块的项目及其初始化  
    如果不带任何参数的克隆，那么子模块会是一个空的文件夹。  
    想要子模块也被正确的初始化，那么请带上 `--recurse-submodules` 选项
    ```
    git clone --recurse-submodules <url>
    ```

    如果忘了带上这个参数，那么执行下面的这个命令也能正确的初始化
    ```
    git submodule update --init --recursive
    ```

4. 更新子模块  
    更新每个子模块：
    ```
    git submodule update --remote --merge
    git submodule update --remote --rebase
    ```
    不同的选项采用的是不同的合并方法

5. 删除子模块  
    首先，删除 `.gitmodules` 和 `.git/config` 文件中有关该子模块的内容。  
    随后暂存 `.gitmodules` 的更改：
    ```
    git add .gitmodules
    ```
    从git仓库中删除子模块：
    ```
    git rm --cached <submodule-name>
    rm .git/modules/<submodule-name>
    ```
    最后提交这次修改，就完成了。