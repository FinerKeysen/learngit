# patch使用

> patch中包含了代码修改的部分，提交者的信息，message说明等必要信息

在开发中，我们会碰到需要将自己的修改提供给别人但是不提交该修改，此时可以用patch的方式来交换代码；

在开源社区中，这种方式也非常常见，开发者将patch发送给开源社区的维护者或者源代码持有者来评估，如果通过，便能快速的应用该patch。



常见用法

- create patch

```shell
# git format-patch

  $ git format-patch HEAD^						#生成最近的1次commit的patch
$ git format-patch HEAD^^							#生成最近的2次commit的patch
$ git format-patch HEAD^^^						#生成最近的3次commit的patch
$ git format-patch HEAD^^^^						#生成最近的4次commit的patch
$ git format-patch <r1>..<r2>					#生成两个commit间的修改的patch（包含两个commit. <r1>和<r2>都是具体的commit号)
$ git format-patch -1 <r1>						#生成单个commit的patch
$ git format-patch <r1>								#生成某commit以来的修改patch（不包含该commit）
$ git format-patch --root <r1>				#生成从根到r1提交的所有patch
```



- apply patch

```shell
# git am
$ git apply --stat 0001-limit-log-function.patch	# 查看patch的情况
$ git apply --check 0001-limit-log-function.patch	# 检查patch是否能够打上，如果没有任何输出，则说明无冲突，可以打上
# (注：git apply是另外一种打patch的命令，其与git am的区别是，git apply并不会将commit message等打上去，打完patch后需要重新git add和git commit，而git am会直接将patch的所有信息打上去，而且不用重新git add和git commit,author也是patch的author而不是打patch的人)
$ git am 0001-limit-log-function.patch	# 将名字为0001-limit-log-function.patch的patch打上
$ git am --signoff 0001-limit-log-function.patch	# 添加-s或者--signoff，还可以把自己的名字添加为signed off by信息，作用是注明打patch的人是谁，因为有时打patch的人并不是patch的作者
$ git am ~/patch-set/*.patch	# 将路径~/patch-set/*.patch 按照先后顺序打上
$ git am --abort		# 当git am失败时，用以将已经在am过程中打上的patch废弃掉(比如有三个patch，打到第三个patch时有冲突，那么这条命令会把打上的前两个patch丢弃掉，返回没有打patch的状态)
$ git am --resolved	#当git am失败，解决完冲突后，这条命令会接着打patch
```



