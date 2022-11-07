# Linux使用

## tmux和vim

### tmux教程

1. 功能：

- 分屏。

- 允许断开Terminal连接后，继续运行进程。

2. 结构：

- 一个tmux可以包含多个session，一个session可以包含多个window，一个window可以包含多个pane。
      实例：
          tmux:
              session 0:
                  window 0:
                      pane 0
                      pane 1
                      pane 2
                      ...
                  window 1
                  window 2
                  ...
              session 1
              session 2
              ...

3. 操作

- tmux：新建一个session，其中包含一个window，window中包含一个pane，pane里打开了一个shell对话框。
- 按下Ctrl + a后手指松开，然后按%：将当前pane左右平分成两个pane。
- 按下Ctrl + a后手指松开，然后按"（注意是双引号"）：将当前pane上下平分成两个pane。
- Ctrl + d：关闭当前pane；如果当前window的所有pane均已关闭，则自动关闭window；如果当前session的所有window均已关闭，则自动关闭session。
- 鼠标点击可以选pane。
- 按下ctrl + a后手指松开，然后按方向键：选择相邻的pane。
- 鼠标拖动pane之间的分割线，可以调整分割线的位置。
- 按住ctrl + a的同时按方向键，可以调整pane之间分割线的位置。
- 按下ctrl + a后手指松开，然后按z：将当前pane全屏/取消全屏。
- 按下ctrl + a后手指松开，然后按d：挂起当前session。
- tmux a：打开之前挂起的session。
- 按下ctrl + a后手指松开，然后按s：选择其它session。
      方向键 —— 上：选择上一项 session/window/pane
      方向键 —— 下：选择下一项 session/window/pane
      方向键 —— 右：展开当前项 session/window
      方向键 —— 左：闭合当前项 session/window
- 按下Ctrl + a后手指松开，然后按c：在当前session中创建一个新的window。
- 按下Ctrl + a后手指松开，然后按w：选择其他window，操作方法与session完全相同。
- 按下Ctrl + a后手指松开，然后按PageUp：翻阅当前pane内的内容。
- 鼠标滚轮：翻阅当前pane内的内容。
- 在tmux中选中文本时，需要按住shift键。（仅支持Windows和Linux，不支持Mac，不过该操作并不是必须的，因此影响不大）
- tmux中复制/粘贴文本的通用方式：
      按下Ctrl + a后松开手指，然后按[
      用鼠标选中文本，被选中的文本会被自动复制到tmux的剪贴板
      按下Ctrl + a后松开手指，然后按]，会将剪贴板中的内容粘贴到光标处

### vim操作

#### 功能

1. 命令行模式下的文本编辑器。
2. 根据文件扩展名自动判别编程语言。支持代码缩进、代码高亮等功能。
3. 使用方式：vim filename。如果已有该文件，则打开它；如果没有该文件，则打开个一个新的文件，并命名为filename

#### 模式

1. 一般命令模式
   默认模式。命令输入方式：类似于打游戏放技能，按不同字符，即可进行不同操作。可以复制、粘贴、删除文本等。
2. 编辑模式
   在一般命令模式里按下i，会进入编辑模式。
   按下ESC会退出编辑模式，返回到一般命令模式。
3. 命令行模式
   在一般命令模式里按下:/?三个字母中的任意一个，会进入命令行模式。命令行在最下面。
   可以查找、替换、保存、退出、配置编辑器等。

#### 操作

- i：进入编辑模式
- ESC：进入一般命令模式
- h 或 左箭头键：光标向左移动一个字符
- j 或 向下箭头：光标向下移动一个字符
- k 或 向上箭头：光标向上移动一个字符
- l 或 向右箭头：光标向右移动一个字符
- n<Space>：n表示数字，按下数字后再按空格，光标会向右移动这一行的n个字符
- 0 或 功能键[Home]：光标移动到本行开头
- \$ 或 功能键[End]：光标移动到本行末尾
- G：光标移动到最后一行
- :n 或 nG：n为数字，光标移动到第n行
- gg：光标移动到第一行，相当于1G
- n<Enter>：n为数字，光标向下移动n行
- /word：向光标之下寻找第一个值为word的字符串。
- ?word：向光标之上寻找第一个值为word的字符串。
- n：重复前一个查找操作
- N：反向重复前一个查找操作
- :n1,n2s/word1/word2/g：n1与n2为数字，在第n1行与n2行之间寻找word1这个字符串，并将该字符串替换为word2
- :1,\$s/word1/word2/g：将全文的word1替换为word2
- :1,\$s/word1/word2/gc：将全文的word1替换为word2，且在替换前要求用户确认。
- v：选中文本
- d：删除选中的文本
- dd: 删除当前行
- y：复制选中的文本
- yy: 复制当前行
- p: 将复制的数据在光标的下一行/下一个位置粘贴
- u：撤销
- Ctrl + r：取消撤销
- 大于号 >：将选中的文本整体向右缩进一次
- 小于号 <：将选中的文本整体向左缩进一次
- :w 保存
- :w! 强制保存
- :q 退出
- q! 强制退出
- :wq 保存并退出
- :set paste 设置成粘贴模式，取消代码自动缩进
- :set nopaste 取消粘贴模式，开启代码自动缩进
- :set nu 显示行号
- :set nonu 隐藏行号
- gg=G：将全文代码格式化
- :noh 关闭查找关键词高亮
- Ctrl + q：当vim卡死时，可以取消当前正在执行的命令
- 异常处理：
      每次用vim编辑文件时，会自动创建一个.filename.swp的临时文件。
      如果打开某个文件时，该文件的swp文件已存在，则会报错。此时解决办法有两种：
          (1) 找到正在打开该文件的程序，并退出
          (2) 直接删掉该swp文件即可

## shell

### 概述

1. 脚本示例

```bash
#! /bin/bash
echo "Hello World!"
```

## ssh

------

### 基本使用

------

1. 远程登录服务器

```sh
ssh user@hostname
```

- `user`：用户名
- `hostname`：IP地址或域名

2. 退出输入`logout`	或者ctrl + d

3. 如果需要指定端口，则需要使用`-p`

```shell
ssh user@hostname -p 22
```

### 配置文件

------

创建文件在`~/.ssh/config`

```shell
Host myserver
    HostName IP地址或域名
    User 用户名
```

之后再使用服务器时，可以直接使用别名`myserver`

### 密钥登录

------

创建密钥：

```shell
ssh-keygen
```

执行结束后，~/.ssh/目录下会多两个文件：

- `id_rsa`：私钥（不能给任何人看）
- `id_rsa.pub`：公钥

想免密登录`myserver`服务器。则将公钥中的内容，复制到`myserver`中的`~/.ssh/authorized_keys`文件里即可。

也可以使用`ssh-copy-id myserver`一键添加公钥

### 执行命令

------

```sh
ssh user@hostname command
```

### scp

------

命令格式：

```shell
scp source destination
```

将`source`文件复制到`destination`

一次性复制多个文件：

```shell
scp source1 source2 destination
```

复制文件夹：

```shell
scp -r source destination
```

指定服务器的端口号：

```shell
scp -P 22 source1 source2 destination
```

## git使用

### git基本概念

- 工作区：仓库的目录。工作区是独立于各个分支的。
- 暂存区：数据暂时存放的区域，类似于工作区写入版本库前的缓存区。暂存区是独立于各个分支的。
- 版本库：存放所有已经提交到本地仓库的代码版本
- 版本结构：树结构，树中每个节点代表一个代码版本。

### git常用命令

```git config --global user.name xxx``` ：设置全局用户名

```git config --global user.name xxx```：设置全局邮箱地址

```git init``` 将当前目录配置成git仓库，信息记录在隐藏的.git文件夹中

```git add x```：将XX文件添加到暂存区

```git rm --cached xx```：将文件从仓库索引目录中删掉

```git commit -m "xxx"```：将暂存区的内容提交到当前分支

```git status```：查看仓库状态

```git diff XX```：查看XX文件相对于暂存区修改了哪些内容

```git log```：查看当前分支的所有版本

```git reflog```：查看HEAD指针的移动历史（包括被回滚的版本）

```git reset --hard HEAD^``` 或``` git reset --hard HEAD~```：将代码库回滚到上一个版本

```git reset --hard HEAD^^```：往上回滚两次，以此类推

```git reset --hard HEAD~100```：往上回滚100个版本

```git reset --hard``` 版本号：回滚到某一特定版本

```git checkout — XX或git restore XX```：将XX文件尚未加入暂存区的修改全部撤销

```git remote add origin git@git.acwing.com:xxx/XXX.git```：将本地仓库关联到远程仓库

```git push -u``` (第一次需要-u以后不需要)：将当前分支推送到远程仓库

```git push origin branch_name```：将本地的某个分支推送到远程仓库

```git clone git@git.acwing.com:xxx/XXX.git```：将远程仓库XXX下载到当前目录下

```git checkout -b branch_name```：创建并切换到branch_name这个分支

```git branch```：查看所有分支和当前所处分支

```git checkout branch_name```：切换到branch_name这个分支

```git merge branch_name```：将分支branch_name合并到当前分支上

```git branch -d branch_name```：删除本地仓库的branch_name分支

```git branch branch_name```：创建新分支

```git push --set-upstream origin branch_name```：设置本地的branch_name分支对应远程仓库的branch_name分支

```git push -d origin branch_name```：删除远程仓库的branch_name分支

```git pull```：将远程仓库的当前分支与本地仓库的当前分支合并

```git pull origin branch_name```：将远程仓库的branch_name分支与本地仓库的当前分支合并

```git branch --set-upstream-to=origin/branch_name1 branch_name2```：将远程的branch_name1分支与本地的branch_name2分支对应

```git checkout -t origin/branch_name``` 将远程的branch_name分支拉取到本地

```git stash```：将工作区和暂存区中尚未提交的修改存入栈中

```git stash apply```：将栈顶存储的修改恢复到当前分支，但不删除栈顶元素

```git stash drop```：删除栈顶存储的修改

```git stash pop```：将栈顶存储的修改恢复到当前分支，同时删除栈顶元素

```git stash list```：查看栈中所有元素

## thrift

