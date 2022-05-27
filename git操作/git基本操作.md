# git操作

[toc]

## 配置用户信息

```
git config --global user.name brokyz
git config --global user.email brokyz@foxmail.com
```

## git命令

### git常用命令

```git
# 设置用户签名
git config --global user.name boqiangZ
# 设置用户邮箱
git config --global user.email boqiangZ@foxmail.com
# 初始化本地仓库 默认分支为master
git init
# 查看本地仓库状态
git status
# 将文件添加到暂存区 .代表当前目录下的所有文件
git add .
# 提交到本地仓库 -m后可以附带日志注释信息，文件名不写默认提交所有暂存区
git commit -m '注释信息' 文件名
# 合并add和commit命令
git commit -am '注释'
# 查看历史记录
git reflog
# 版本穿梭 回到某个操作
git reset --hard 版本号
```

### git分支操作

```git
# 创建分支
git branch 分支名
# 查看分支
git branch -v
# 切换分支
git checkout 分支名
# 将指定分支合并到当前分支
git merge 分支名
# 删除分支 -D为强制删除
git branch -d 分支名
```

### git远端仓库操作

```git
# 查看当前所有远程地址别名
git remote -v
# 添加远程仓库并设置别名
git remote add 别名 远程地址
# 推送本地分支上的内容到远程仓库 加-f参数强制推送，覆盖远端
git push 别名 分支
# 推送所有分支
git push 别名 --all
#将远程仓库的内容克隆到本地
git clone 远程地址
# 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并
git pull 远程库地址别名 远程分支名
```

## git解决冲突

**冲突的产生原因**：当两个人同时对一个文件的同一个位置进行修改时，提交的时候就会存在冲突，比如：一开始 a = 0，人员1修改了 a = 2，人员2修改了 a = 3。这时提交时，这由于人员1，从 a = 0 修改到 a = 1；人员2，也从 a = 0，修改到 a = 3。于是两个人的修改就存在了冲突，无法决定最终保留哪个人的修改。

**解决冲突**：当两个人同时修改同一个文件的同一个位置时，最后一个提交的人会出现产生冲突的提示。这时，git会以冲突提示的形式，同时在文件中保留两个人的修改。以上面的人员1和人员2为例，如果人员1先提交，人员1，提交正常。人员2后提交，人员2就会提示，提交的修改存在冲突。这是在文件中会同时两个人修改的信息 a = 2 和 a = 3，但是会将这个冲突修改以特殊符号包裹，来让提交人员进行选择性保留。这时候，提交人员只需要在文件中删除不需要保留的修改和特殊符号，重新提交即可。文件中冲突的保留样式如下所示：

```git
<<<<<<< HEAD
a = 2
=======
a = 3
>>>>>>> crash 
# 如果要保留 a = 3 只需要删除其他特殊符号，只保留a = 3即可，如下：
a = 3
# 之后保存文件，重新提交即可
```

## git添加远程仓库许可(以github为例)

如果需要将本地仓库的修改推送到GitHub，就需要GitHub账号认证我们本地的机器，这样才允许本地机器push到远端。

1. 首先需要在本地机器生成本地密钥和公钥

```git
# 进入当前用户文件夹，删除.ssh文件
# 生成密钥
ssh-keygen -t rsa -C brokyz@foxmail.com
# 提示全部回车
```

2. 其中生成的id_rsa.pub文件里面的内容就是密钥。

```
# 查看密钥
cat ~/.ssh/id_rsa.pub
```

2. 将密钥中的内容添加到GitHub账户中Settings -> SSH and GPG keys -> New SSH key
4. 操作完成后即可push到GitHub

## git.ignore忽略文件

ignore文件可以控制git哪些文件可以忽略上传到远端

ignore文件的前缀随便起名，但是建议起名为git.ignore

ignore文件的存放位置也可以位于任意处，但是为了便于让~/.gitconfig 文件引用，建议也放在用户家目录下

**git.ignore模板**

```gitignore
# Log file
*.log
# BlueJ files
*.ctxt
# Mobile Tools for Java (J2ME)
.mtj.tmp/
# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
# virtual machine crash logs, see 
http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.im在.gitconfig 文件中引用忽略配置文件（此文件在 Windows 的家目录中）
```

**在.gitconfig 文件中引用忽略配置文件（此文件在 Windows 的家目录中）**

```
[user]
name = Layne
email = Layne@atguigu.com
[core]
excludesfile = C:/Users/asus/git.ignore
# 注意：这里要使用“正斜线（/）”，不要使用“反斜线（\）
```
