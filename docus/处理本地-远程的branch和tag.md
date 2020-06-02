# git更新本地远程branch与tag

参考：

[git 更新本地分支信息](https://www.jianshu.com/p/83acc1211742)

[git 如何同步本地、远程的分支和tag信息](https://blog.csdn.net/wei371522/article/details/83186077)

## 分支

```sh
git fetch origin --prune
```

该命令适合本地长时间未更新，用以同步远程分支信息。比如你fork了某个远程仓，但是不用于开发，一段时间后，该远程仓更新了很多，你需要保持更新，此时可用该命令，同步后再使用`git push origin 自己的远程仓:本地仓`命令将本地同步到自己fork的仓库。



场景：开发一段时间后，本地遗留有一些无用分支，但很多分支在远程仓库中已删除。此时需要清理本地的无用分支。

1、查看当前本地库与远程库的分支同步情况

```sh
git remote show origin
```

![img](%E5%A4%84%E7%90%86%E6%9C%AC%E5%9C%B0-%E8%BF%9C%E7%A8%8B%E7%9A%84branch%E5%92%8Ctag.assets/18660577-998bf1400b218e34-1591073329120.png)

图片来源：https://www.jianshu.com/p/83acc1211742

2、将仓库中已删除的分支与本地分支的追踪关系删除掉

```
git remote prune origin

# 等同于
git fetch -p
git fetch --prune
git fetch -p origin
```

![img](%E5%A4%84%E7%90%86%E6%9C%AC%E5%9C%B0-%E8%BF%9C%E7%A8%8B%E7%9A%84branch%E5%92%8Ctag.assets/18660577-11d11d2438784092-1591073297208.png)

图片来源：https://www.jianshu.com/p/83acc1211742



## tag

git 如何同步本地tag与远程tag
问题场景：
同事A在本地创建tagA并push同步到了远程->同事B在本地拉取了远程tagA(git fetch)->同事A工作需要将远程标签tagA删除->同事B用git fetch同步远端信息，git tag后发现本地仍然记录有tagA

分析：对于远程repository中已经删除了的tag，即使使用git fetch --prune，甚至"git fetch --tags"确保下载所有tags，也不会让其在本地也将其删除的。而且，似乎git目前也没有提供一个直接的命令和参数选项可以删除本地的在远程已经不存在的tag（我目前是没找到有关这类tag问题的git命令~~，有知道的同学可以告知我下，互相进步）。
解决方法：

```sh
git tag -l | xargs git tag -d #删除所有本地分支
git fetch origin --prune #从远程拉取所有信息
```



tag的一些用法

```sh
#查询远程tags的命令如下：
git ls-remote --tags origin

tag常用git命令：
git tag #列出所有tag
git tag -l v1.* #列出符合条件的tag（筛选作用）
git tag #创建轻量tag（无-m标注信息）
git tag -a -m ‘first version’ #创建含标注tag
git tag -a f1bb97a(commit id) #为之前提交打tag

git push origin --tags #推送所有本地tag到远程
git push origin #推送指定本地tag到远程

git tag -d #删除本地指定tag
git push origin :refs/tags/ #删除远程指定tag

git fetch origin #拉取远程指定tag
git show #显示指定tag详细信息
```

