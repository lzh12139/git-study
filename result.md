# git常用操作

## 开始

设置提交时显示的用户名，邮箱：
```
git config --global user.name "zhiruili"
git config --global user.email "zr.public@outlook.com"
```
网络配置：
```
git config --global https.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
```
本文中所有 `<>` 括起来的内容为必选项， `[]` 括起来的内容为可选项。

## 基础操作

1. 创建仓库  
    - 通过 `init` 初始化仓库  
    11

    - 通过 `clone` 初始化仓库  
    22

2. pull和fetch

3. add

4. commit

5. push

## merge 的常用操作方式

## cherry pick

## 版本回滚或 rebase

## branch

## 子模块

1. 定义  
    子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。   
    它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

2. 命令
    ```
    git submodule status -- 当前仓库的子模块的状态，如果子模块的最开始（hash值前）有一个减号，则表明这个子模块
    git submodule add <url> 
    ***[-b branch]***未成功
     [path] -- 增加一个子模块，并将子模块放在传入的路径里。
    git submodule init -- 初始化本地的配置文件
    git submodule update
    ```

    当clone一个拥有子模块的仓库时，默认会包含该子模块目录，但其中没有任何文件。
    可以通过执行 init+update