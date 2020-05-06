# git cherry-pick usage



**git cherry-pick**可以选择某一个分支中的一个或几个commit(s)来进行操作。

例如，假设我们有个稳定版本的分支，叫master，另外还有个开发版本的分支dev，我们不能直接把两个分支合并，这样会导致稳定版本混乱，但是又想增加一个dev中的功能到master中，这里就可以使用cherry-pick了,其实也就是对已经存在的commit 进行再次提交.

**用法**

`git cherry-pick <commit id>`
**注意：当执行完 cherry-pick 以后，将会生成一个新的提交；这个新的提交的哈希值和原来的不同，但标识名一样；**

先在dev分支中提交

```
git add .
git commit -m "message"

# 查看分支中提交的信息
git log
```

然后转到master分支，cherry-pick这个提交（假设这次提交的id号码为 123a123b）

```
git checkout master

git cherry-pick 123a123b
# 此时会产生一个新的提交号码，假设为456a456b
```

1、如果没有conflicts，就会正常提交，然后就可以push你的代码到远程仓库；

或者产生的改变在你的许可范围内，你也可以继续push



2、如果cherry-pick 过程中出现了冲突

这时你要看哪些文件出现冲突

```
git status
```

然后解决冲突

```
vim 冲突的文件
```

然后再add 这个冲突文件

```
git add 解决的冲突文件
```

再将修改的冲突提交到到之前的commit中

```
git commit -c 456a456b
# -c可以重新编辑提交注释
```

然后再 cherry-pick 这个提交

```
git cherry-pick 456a456b
```

