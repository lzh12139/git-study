# git常用操作

## 开始

设置提交时显示的用户名，邮箱：
```
git config --global user.name "xxx"
git config --global user.email "xxx@xxx.com"
```
网络配置：
```
git config --global https.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
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

2. 暂存文件
   ```
   git add <file>
   ```
   将文件添加到暂存区。

3. 提交更新
   ```
   git commit -m "<message>"将暂存区中的文件提交到版本库中，可以一次提交多个文件
   git commit -a 将已追踪所有的文件暂存，通常搭配 -m 一起用
   ```

4. 提交到远程仓库
    ```
    git push <远程主机名> <本地分支名>:<远程分支名>
    ```
    将本地分支的更新，推送到远程主机。
    
    可以使用 `-u` 选项指定默认主机，这样以后就可以不加任何参数地使用 `git push` 了。

5. pull  
    将远程仓库的最新更改合并到当前分支中，相当于 `fetch+merge`
    ```
    git pull [主机名] [分支名]
    ```
    不推荐这样做，建议 `fetch+merge`

6. fetch  
    用于取回远程仓库所有分支的最新快照，并保存在版本库中。
    ```
    git fetch [主机名]  创建并更新所有远程分支的本地远程分支，FETCH_HEAD是字典序最小的那个分支？
    git fetch <远程主机名> <分支名> 仅拉取对应的分支，FETCH_HEAD等于这个分支
    ```
    拉取之后，可以用 `origin/<分支名>` 来代表最新的分支。

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
    git diff --staged               比较暂存区和版本库的差异。
    git diff <file>                 比较这个文件的工作区与暂存区的差异。
    git diff <branch1> <branch2>    比较两个分支的差异。
    git diff <SHA1> <SHA2>          比较两次提交的差异。
    ```

10. log
    ```
    git log -p              按补丁格式显示每个提交引入的差异。  
    git log --graph         在日志旁以 ASCII 图形显示分支与合并历史。 
    git log --follow <file> 该文件的改动，包括重命名
    git log <SHA>           查询该提交之前的记录，包含该提交
    git log <SHA1> <SHA2>   查询 提交1 与 提交2 之间的记录，包括提交1 与 提交2 
    git log <SHA1>..<SHA2>  查询 提交1 与 提交2 之间的记录，只包括提交1
    ``` 

11. stash  
    https://www.yiibai.com/git/git_stash.html
- [ ] 在看了再看了(0%)

12. rm 
    ```
    git rm <file> 同时删除磁盘上和暂存区上的文件
    git rm --cached <file>仅删除暂存区上的文件
    ```  
    将文件从已跟踪文件清单中移除需要使用上面的命令，而不是直接在文件目录中删除该文件。这样会删除git仓库中和磁盘上的该文件。  
    注意，如果要将该文件从git仓库中删除的话，不能只删除本地文件

13. 远程仓库服务器
    ```
    git remote -v   查看配置的远程仓库服务器。
    git remote --add
    git remote set-url
    ```
    查看配置的远程仓库服务器。  
    以git开头的使用SSH公钥认证

14. 密钥  
    可以使用 SSH公钥 来进行认证，将公钥传到 github 上后push操作就不需要输入用户名和密码。  

    注意，只有在使用 SSH协议 时，才会通过 SSH公钥 来认证。

    ***注意：不确定是否在公司电脑上可行，未尝试过，只确定在github上可行***  

    - [ ] 上 8000 确定

    执行
    ```
    ssh-keygen
    ```
    然后按三次回车，将 `~/ .ssh/id_rsa.pub` 的内容复制到 github 个人设置的 `SSH and GPG keys` 里面。

    使用下面的命令查看是否成功
    ```
    ssh -T git@github.com
    ```
    如果出现了用户名，说明成功了。

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

## merge 的常用操作方式

https://www.yiibai.com/git/git_merge.html

## cherry-pick 和 rebase

rebase https://mp.weixin.qq.com/s/Y-imESQiYRCZXJd_mkkoZw

## 版本回滚
1. 版本  
    记**本地的版本库**的当前版本为 `HEAD`   
    前一个版本为 `HEAD^` ，前两个版本为 `HEAD^^` ，  
    前100个版本为 `HEAD~100`  
    在执行 `reset` , `pull` , `merge` 操作之后，会把操作前的 `HEAD` 放入 `ORIG_HEAD` 中，以防回滚操作

2. 撤销暂存
    ```
    git reset HEAD <file>
    ```

3. 回退版本
    ```
    git reset --hard <SHA | HEAD~n>
    ```
    强制放弃所有更改，并**回退到**指定版本

4. 回退commit
    ```
    git reset --soft <SHA | HEAD~n>
    ```
    撤销 commit ，但是不会撤销文件的更改。  
    可以用来保存当前分支的修改，然后切换到另一分支。  

    常用方法： 刚刚 commit 错了，执行
    ```
    git reset --soft HEAD^
    因为刚刚的commit错了，而HEAD指向最新的commit
    所以要回退到HEAD的前一个版本，也就是HEAD^
    ```
    更改的文件会保留在暂存区中

5. 撤销某个commit
    ```
    git revert <SHA | HEAD~n>    撤销后自动commit，不建议
    git revert -n <SHA | HEAD ~n> 加上选项-n(或--no-commit)，不会自动commit而是停留在工作区
    ```
    相当于做一次所选择的 commit 的逆操作，  
    即如果原 commit 是插入，那么这次就是删除。

6. 在污染的工作区中回滚合并或者拉取  
    git reset --merge
    没看懂  
    https://www.yiibai.com/git/git_reset.html
    - [ ] 看懂

7. 提交历史  
    ```
    git log     查看所有提交过的版本信息
    git reflog  查看所有操作记录，包括被回滚过的操作
    ```

## branch

https://www.yiibai.com/git/git_branch.html
https://www.yiibai.com/git/git_checkout.html

## 子模块

1. 定义  
    子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。   
    它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

2. 命令
    ```
    git submodule status -- 当前仓库的子模块的状态，如果子模块的最开始（hash值前）有一个减号，则表明这个子模块
    git submodule add <url> [-b branch] [path] -- 增加一个子模块，可指定分支，并将子模块放在传入的路径里。
    git submodule init -- 初始化本地的配置文件
    git submodule update
    ```

    当clone一个拥有子模块的仓库时，默认会包含该子模块目录，但其中没有任何文件。
    可以通过执行 init+update