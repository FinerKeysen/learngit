## 仓库迁移

> 将一个仓库的某一分支迁移到另一个仓库中，包括提交记录等完整信息

场景：

仓库`A`的地址为`urlA`,仓库B的地址为`urlB`，将`A`中的分支`master`迁移至`B`的`master`



### 迁移时，保留旧仓库提交记录

1、拉取`A`的最新代码

```shell
# new 
(master)$ git clone urlA 
# 或者本地已拉分支时
(master)$ git pull
```

假设`A`有分支`b1`、`b2`，需要将`b1`完整推送到`B`的`master`

2、修改`A`的remote地址

```shell
# 修改仓库A的 .git/config 文件
(master)$ vim path/to/A/.git/config

# 将url由urlA修改为urlB
[remote "origin"]
	url = urlB
```

以下操作中可能涉及到输入仓库B的用户及密码登陆

- A与B属于同一账户的不同仓库，不涉及登陆问题
- A与B由不同的账户创建的仓库，需要用新仓库用户及密码登陆

3、推送`b1`到`B`的`master`

```shell
# 先切换到分支b1
(master)$ git checkout b1

# 推送
(b1)$ git push origin b1:master
# 这句的意思是，将本地分支b1推送到远程仓库origin的master分支，我们知道origin已经通过修改url绑定到仓库A
# 但是可能会出现以下错误
```

可能报错

```shell
To urlB
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'urlB'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

提示我们先`git pull`再`git push`

3.1、先pull

```shell
(b1)$ git pull origin master
From http://10.142.233.181:8989/ctgtomcat/tomcat-jdbc
 * branch            master     -> FETCH_HEAD
fatal: refusing to merge unrelated histories

```

出现`refusing to merge unrelated histories`错误，通过`--allow-unrelated-histories`忽略无相关的历史记录解决

```shell
# 重新执行
(b1)$ git pull origin master --allow-unrelated-histories
From http://10.142.233.181:8989/ctgtomcat/tomcat-jdbc
 * branch            master     -> FETCH_HEAD
Already up to date!
Merge made by the 'recursive' strategy.
```

3.2、再push

```shell
(b1)$ git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master

```

设置推送的分支

```shell
# 重新执行
(b1)$  git push --set-upstream origin master
Enumerating objects: 232, done.
Counting objects: 100% (232/232), done.
Delta compression using up to 8 threads
Compressing objects: 100% (78/78), done.
Writing objects: 100% (231/231), 127.67 KiB | 21.28 MiB/s, done.
Total 231 (delta 62), reused 227 (delta 62), pack-reused 0
remote: hukai have be authored all branches.
To urlB
   d06ca95..58c4e47  b1 -> master
Branch 'b1' set up to track remote branch 'master' from 'origin'.

```

`b1`成功完整推送到`B`



### 迁移时，不保留旧仓库的提交记录

- 将旧仓库的文件直接copy到新仓库中
- git commit -m "message..."
- git push