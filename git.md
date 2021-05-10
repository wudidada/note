# git使用

## git工作流

### 拉取分支

#### 本地切换分支

```
git checkout -b [branch]		# 创建并切换到分支
清理上一步中提取中的句子
```

等于

```sh
git branch <branch>
git checkout <branch>
```

#### 拉取远程到本地分支

```
git fetch origin <branch-name>:<local-branch-name>		# branch-name留空则为master
```

### 提交分支

```
git remote [-v] 		# 查看远程
git push <remote> <local-branch-name>:<branch-name>
```

### 合并分支到主干

在网页上操作合并，一般合并后需要删除分支

### 删除本地分支

```
git branch (-d | -D) [-r] <branchname>…
```

-D	强制删除分支

-r	(--romotes)	配合-d删除本地的远程分支

**加入这个选项是否就无需再同步远程的分支信息了？**

### 同步远程分支信息

由于网页上操作删除了远程的分支，需要同步到本地，否则本地的分支信息中依然保存有远程的分支信息。