# commit 提交多行信息

**方式一**

连续的`-m`提交信息

```sh
git commit -m "the message 1" -m "the message 2"
```

得到的

```sh
Author: ......
Date: .....
	the message 1
	
	the message 2
.....
```



**方式二**

半开方式，中间隔断

```sh
git commit -m "the message 1
>
> the message 2"
```

得到

```sh
Author: ......
Date: .....
	the message 1
	
	the message 2
.....
```



**方式三**

在idel上，左侧栏的Commit管理栏，选择文件后，在信息框中输入多行信息（换行隔断），提交

可得到

```sh
Author: ......
Date: .....
	the message 1
	the message 2
.....
```

