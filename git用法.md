## 创建仓库	

​		在创建的目录里使用命令	**git init**  就以这个目录创建了一个仓库，其中包含 **.git** 隐藏目录代表这是一个git仓库，如果将这个  **.git** 隐藏文件删除 **rm -rf  .git**，则仓库也会被删除，此时仅代表一个目录。

```
git init     以当前目录创建一个仓库
git init ..repo 在指定位置创建仓库。
```

## git的工作区域和文件状态

​		git的本地数据管理分为三个工作区域：工作区、暂存区和本地仓库

```
工作区(working directory)：工作目录/本地工作目录
暂存区(staging area/indx)：临死存储区域，用于保存即将提交到git仓库的内容
本地仓库(local repository): 通过git init创建的仓库，包含了完整的项目历史和元数据。
```

![image-20240516212056008](F:/md%E6%96%87%E4%BB%B6/images/image-20240516212056008-17158656582951.png)

文件状态：未跟踪(Untracked)、暂存状态(staged)、已修改(modified)、已提交(committed)

```
Untracked：这些文件存在于工作目录中，但是还没有被添加到git仓库的暂存区，即在工作区新创建的文件，git并不知道这些文件的存在 通过 git add <file> 命令将文件添加到暂存区，从而使文件状态变成已暂存状态。
staged:这些文件已经被添加到暂存区(也称索引区)，等待被提交。git add <file> 命令可以将修改后的文件从工作目录添加到暂存区。而 git restore --staged <file> 用于从暂存区移除文件，仅影响暂存区，而不影响工作区的文件。
modified: 文件已经被修改，但是没有添加到暂存区，使用git add <file> 将修改添加到暂存区；也可以使用 git check -- <file> 命令撤销对工作中文件的修改，使其恢复到上一次提交的状态。前提是没有在暂存修改，如果已经将修改通过 git add 将修改暂存，则需要先将暂存中的修改文件移除，再恢复。
committed:这些文件已经被提交到git仓库,存储在git仓库的历史记录中  通过 git commit -m "提交信息" 命令将文件提交到本地仓库。 而提价的文件可以通过 git log 命令查看提交历史。

git status 查看创库状态
git add  添加到暂存区
git commit 将暂存区中的内容提交到仓库
git log 查看提交历史  
```

## git reset 命令回退版本的三种模式

```
git reset --soft <commit>
git reset --hard <commit>
git reset --mixed <commit>

--soft 模式：
	作用：将当前分支指向指定的提交，同时保留暂存区和工作区的更改。
	示例：假设你有三个提交A、B、C ，目前在C上，运行 git reset --soft B， 当前分支会回到B，但是在C中的更改会保留到暂存区。

--mixed 模式（默认模式）
	作用：将当前分支指向指定的提交，同时重置暂存区（staging area），但保留工作目录（working directory）的更改。
	示例：假设你有三个提交 A、B、C，目前你在 C 上。如果你运行 git reset --mixed B，当前分支会回到 B，C 中的更改会被移出暂存区，但保留在工作目录中（未暂存状态）。
	
--hard模式
	作用：将当前分支、暂存区（staging area）和工作目录（working directory）都重置到指定的提交。
	示例：假设你有三个提交 A、B、C，目前你在 C 上。如果你运行 git reset --hard B，当前分支会回到 B，C 中的所有更改会被丢弃，无论是暂存的还是未暂存的。
```

## git diff 查看差异

```cpp
·查看工作区、暂存区、本地仓库之间的差异
·查看不同版本之间的差异
·查看不同分支之间的差异
//
```

```cpp
git config --global user.email=...
//git config --local user.email=...
git config --global user.name=...
//git config --local user.name=...
```

## 从本地仓库删除文件  git rm  指定文件

```bash
git rm <file>  #从暂存区中删除文件
git commit  #提交到本地仓库，删除本地仓库文件

#删除一个本地仓库中的文件，需要删除暂存区中存储的文件，再将其提交，实现删除仓库中的文件。
#删除指定文件夹文件夹
git rm -r path/to/folder
git commit -m "delete folder"

git rm --cached <file>  #把文件从暂存区（已经跟踪的文件）中删除，但保留在当前工作区，如果从暂存区中执行，并执行git commit，将会把仓库中的文件删除，而不会删除工作区中的文件。

ls   #列出工作目录中的文件
git ls-files  #列出git仓库中所有已跟踪的文件

#列出当前分支中的所有文件
git ls-tree -r HEAD --name-only
```

## .gitignore

```cpp
.gitignore文件用于告诉Git在版本控制中忽略那些文件和目录。//避免将不必要的文件（编译生成的文件、临时文件、特定环境的配置文件等）添加到仓库中。
语法：
  	·以#开头的为注释行
    ·空行作为分隔符，被忽略
    ·文件名或目录名为要忽略的文件或目录
    ·支持通配符
    	*：匹配零个或多个字符
    	？：匹配一个字符
    	**：匹配零个或多个目录
    ·以！开头的行表示不忽略匹配的文件或目录，即使之前已经定义规则。
    
```

## 本地仓库如何与远程仓库关联

```bash
#本地创建新的仓库，并关联到远程仓库
    # 创建 README 文件并初始化 Git 仓库
    echo "# local-remote_link" >> README.md
    git init

    # 添加文件到暂存区并提交
    git add README.md
    git commit -m "first commit"

    # 重命名当前分支为 main
    git branch -M main

    # 添加远程仓库地址
git remote add origin git@github.com:thinker000/local-remote_link.git  #远程仓库地址

    # 将本地仓库的内容推送到远程仓库
    git push -u origin main # 将本地的main分支推送到远程仓库的main分支，并设置该分支的上游分支（这样以后可以直接 git push 或 git pull）

git push -u origin feature:feature-remote
#推送本地分支 feature 到远程并命名为 feature-remote

git branch --set-upstream-to=origin/feature-remote feature    #将本地分支feature与远程分支建立关联，并设置feature的上游分支为远程origin/feature-remote分支


如果是现有的地址则直接执行 git remote add origin ...


git remote -v  #用于显示当前Git仓库配置的远程仓库的名称及其对应的URL.显示获取(fetch)和推送(push)URL   
##    格式: 远程仓库名 URL
```



## 本地与远程工作的整体流程

```cpp
1.本地修改仓库中文件后，在本地仓库提交之后git cimmit，需要从远程仓库获取最新的更改但不合并；  
2.从远程仓库获取最新的更改但不合并；git fetch origin
	// 查看获取到的更改； git log origin
3.将获取到的最新更改合并到当前分支；git merge origin
	//当 git merge 合并分支时，如果在同一个文件的用一个位置存在不同的更改，就会产生冲突。此时Git无法自动合并这些冲突，需要手动解决
/* 冲突解决
1.通过 git status;查看哪些文件存在冲突
2.打开冲突文件：冲突标记格式
				<<<<<<< HEAD
				你的更改
				=======
				分支的更改
				>>>>>>> branch-name
3.手动解决冲突：即手动编辑文件，决定哪些部分保留，解决冲突后，删除文件中的冲突标记（<<<<<<, =======, >>>>>>）
4.标记冲突已解决： git add <冲突文件名>
5.提交合并结果： git commit
*/
//git pull origin 相当于 git fetch origin + git merge origin  但是在发生冲突时还需要手动修改,通过git status 查看是否有冲突   冲突信息:[both modified ]
4.将本地更改/或合并后的更改推送到远程仓库；git push origin 
```

