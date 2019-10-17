# 分支使用方法

## 创建分支

```
# 创建 dev 分支
git branch dev
```

## 切换分支

```
# 切换到 dev 分支
git checkout dev

# 创建并切换到 dev 分支
git checkout -b dev
```

## 合并分支

假设需要将 dev 分支合并到 master 分支

```
# 先切回 master
git checkout master
# 再合并 dev 分支到 当前分支(master)
git merge dev
```

## 删除本地分支

```
# 删除 dev 分支
git branch -d dev
```

## 查看分支列表

```
git branch

# 查看所有分支的最后一次操作
git branch -v

# 查看当前分支
git branch -vv
```

