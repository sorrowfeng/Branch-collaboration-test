# 分支协作测试工程

> 文章详解 [如何使用Git提高研发团队工作效率](https://zhuanlan.zhihu.com/p/55602151)




## 目录

* [多分支 同仓库 工作流程](#多分支-同仓库-工作流程)
	* [克隆远端仓库](#克隆远端仓库)
	* [main分支的添加与提交操作](#main分支的添加与提交操作)
	* [创建新分支进行工作的流程](#创建新分支进行工作的流程)
		* [拉取](#拉取)
		* [创建并切换分支](#创建并切换分支)
		* [在分支上进行代码修改](#在分支上进行代码修改)
		* [合并到主分支](#合并到主分支)
		* [推送到远端](#推送到远端)
* [Fork 主仓库通过 Pull Request 协作流程](#Fork-主仓库通过-Pull-Request-协作流程)
	* [Fork 主仓库](#Fork-主仓库)
	* [Clone 远程仓库](#Clone-远程仓库)
	* [创建新的分支](#创建新的分支)
	* [编辑代码](#编辑代码)
	* [提交更改](#提交更改)
	* [推送分支到远程仓库](#推送分支到远程仓库)
	* [创建 Pull Request](#创建-Pull-Request)
	* [审查和合并 Pull Request](#审查和合并-Pull-Request)
	* [更新本地仓库](#更新本地仓库)
* [Git记录](#Git记录)
	* [删除远端分支](#删除远端分支)



# 多分支 同仓库 工作流程

## 克隆远端仓库

```cpp
git clone https://github.com/sorrowfeng/Branch-collaboration-test.git
```

## main分支的添加与提交操作

```cpp
git add "readme.md" // 添加文件
git commit -m "commit" // 提交文件到暂存区并添加注释
git push origin main // 推送commit到远端仓库
```

> git commit -a -m "commit" 
>
> `git commit -a` 是一个 Git 命令，它的作用是将所有已跟踪（tracked）但尚未提交（uncommitted）的修改全部提交到本地仓库。
>
> 具体来说，`-a` 参数表示自动将所有已跟踪但尚未提交的修改添加到 Git 的暂存区中，然后执行 `git commit` 命令将这些修改提交到本地仓库中。使用该命令需要注意，它只会将已跟踪的文件提交到本地仓库中，对于未跟踪的文件，需要使用 `git add` 命令将其添加到 Git 的跟踪列表中，然后才能使用 `git commit -a` 命令将其提交。
>
> 需要注意的是，`git commit -a` 命令只会将修改提交到本地仓库中，如果你想将修改推送到远程仓库中，还需要使用 `git push` 命令将本地分支推送到远程分支中。另外，建议在提交前仔细检查修改，确保提交的修改是符合要求的，以避免不必要的错误和问题。

## 创建新分支进行工作的流程

### 拉取

```cpp
git pull // 拉取远程最新内容
git pull origin main
```

### 创建并切换分支

```cpp
git branch dev // 创建dev分支
git checkout dev // 切换到dev分支
// or
git checkout -b dev // 创建并切换到dev分支
```

### 在分支上进行代码修改

```cpp
vim main.cpp // 修改代码
git commit -a -m "dev changed" // 在分支上添加并提交
```

### 合并到主分支

``` cpp
git checkout main // 切换到主分支
git merge dev // 合并dev分支
```

> 如果 `file.txt` 文件出现冲突，Git 会提示你进行冲突解决：
>
> ```
> Auto-merging file.txt
> CONFLICT (content): Merge conflict in file.txt
> Automatic merge failed; fix conflicts and then commit the result.
> ```
>
> 打开 `file.txt` 文件，可以看到类似下面的内容：
>
> ```
> <<<<<<< HEAD
> this is the content on the master branch
> =======
> this is the content on the feature branch
> >>>>>>> dev
> ```
>
> 这表示 `master` 分支和 `dev` 分支对 `file.txt` 文件进行了不同的修改。`<<<<<<< HEAD` 和 `=======` 之间是 `master` 分支的修改内容，`=======` 和 `>>>>>>> dev` 之间是 `dev` 分支的修改内容。
>
> 解决冲突后，手动修改 `file.txt` 文件，将冲突部分修改为正确的内容，然后保存文件。
>
> 在当前正在进行合并操作的分支上进行入下操作，如当前便是在main分支上进行操作。
>
> 使用 `git add` 命令将解决冲突后的文件添加到 Git 暂存区：
>
> ```
> git add file.txt
> ```
>
> 最后，使用 `git commit` 命令提交合并结果：
>
> ```
> git commit -m "merge"
> ```
>
> 在提交时，Git 会自动生成一条合并提交记录，并在提交信息中包含有关合并冲突的信息。提交成功后，合并就完成了。
>
> 如果在解决冲突时遇到问题，可以使用 `git merge --abort` 命令取消合并操作。

### 推送到远端

```cpp
git branch -d dev // 删除dev分支
git push origin main // 推送到远端仓库
```

# Fork 主仓库通过 Pull Request 协作流程

## Fork 主仓库

首先，每个贡献者都需要在 GitHub 上 Fork（派生）主仓库，这将创建一个该贡献者自己的副本。这个副本将作为该贡献者的“远程仓库”，他们将在其中进行更改和提交。

## Clone 远程仓库

然后，贡献者需要将他们的远程仓库克隆到本地计算机上，这可以通过使用 `git clone` 命令来完成。例如：

```
$ git clone git@github.com:<username>/<repo-name>.git
```

## 创建新的分支

贡献者应该在本地仓库中创建一个新的分支来进行更改。这可以通过使用 `git checkout` 命令来完成。例如：

```
$ git checkout -b <new-branch-name>
```

## 编辑代码

在本地仓库中进行更改和编辑，以满足要求。

## 提交更改

贡献者需要将更改提交到其本地仓库中，这可以通过使用 `git add` 和 `git commit` 命令来完成。例如：

```
$ git add .
$ git commit -m "commit message"
```

## 推送分支到远程仓库

贡献者需要将其本地分支推送到其远程仓库中。这可以通过使用 `git push` 命令来完成。例如：

```
$ git push origin <new-branch-name>
```

## 创建 Pull Request

在贡献者的远程仓库中，他们可以创建一个 Pull Request（PR）来将其更改合并到主仓库中。他们可以通过 GitHub 界面或命令行工具完成此操作。

## 审查和合并 Pull Request

维护主仓库的负责人现在可以审查 Pull Request，并将其合并到主仓库中。

## 更新本地仓库

一旦主仓库中的更改被合并，贡献者应该更新其本地仓库以包含最新更改。这可以通过使用 `git pull` 命令来完成。例如：

```
$ git pull origin master
```

# Git记录

## 删除远端分支

```
$ git push origin --delete <branch-name>
```

