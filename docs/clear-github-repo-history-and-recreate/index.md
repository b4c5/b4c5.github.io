# 如何清空github仓库历史提交


# 如何清空github仓库历史提交

相当于把仓库清空，重建。

## 方案一：删除.git，并强制push

**原理：**

删除`.git`目录，重新建立git， 并将本地内容强制push到远程仓库上，实现本地、远程仓库的提交记录清空。

**详细步骤：**

1. `.git`目录保存了所有的提交历史，删除该目录，即删除了git所有的历史提交信息
2. 通过`git init`,重新初始化本地项目
3. 关联github上的项目
   `git remote add origin git@github.com/../....git`
4. 强制提交，覆盖
   `git push -u --force origin master:main`

**代码细节（包含了submodule)：**

```bash
rm -rf .git
rm -rf themes/LoveIt		# 如果有submodule，则把submodule全部删除
git init
git config user.name b4c5
git config user.email blaket@qq.com
git branch -m main			# 与github保持一致，主干分支都叫main

git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
git remote add origin git@github-b4c5:b4c5/b4c5.github.io.git
git add .
git commit -m 'init'
git push -u --force origin main		# 这一步是关键，强制覆盖远程仓库
# 如果仓库没有改名，是master的话，则使用
# git push -u --force origin master:main
# more generally:
# git push <remote> <local branch name>:<remote branch to push into>
```

**参考链接：**

[git-clearHistory](https://gist.github.com/stephenhardy/5470814)

## 方案二：下载孤立版本，并强制push

直接删除`.git`可能会有问题，特别是有submodule的仓库。

```bash
git checkout --orphan newBranch	 # 
git add -A  # Add all files and commit them
git commit
git branch -D master  # Deletes the master branch
git branch -m master  # Rename the current branch to master
git push -f origin master  # Force push master branch to github
git gc --aggressive --prune=all     # remove the old files
```

**参考链接：**

 [Make the current commit the only (initial) commit in a Git repository?](https://stackoverflow.com/questions/9683279/make-the-current-commit-the-only-initial-commit-in-a-git-repository)


