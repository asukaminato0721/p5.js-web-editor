# 准备一个拉取请求

从[p5.js仓库](https://github.com/processing/p5.js)中复制并更新。

当您的代码保持最新时，拉取请求会更容易！您可以使用git rebase命令将您的代码更新以包含其他贡献者的更改。以下是具体步骤。

## 保存和更新

### 保存您的所有更改！
    git status
    git add -u
    git commit


### 了解有关更改的信息
确保您正在追踪p5.js仓库的上游。

    git remote show upstream

如果看到错误提示，则需要将主要p5.js仓库设置为"upstream"上游远程仓库的跟踪分支。这只需要做一次！但如果您再次运行它也不会有任何问题。

    git remote add upstream https://github.com/processing/p5.js-web-editor

然后询问git有关最新更改的信息。

    git fetch upstream

### 以防万一：在新分支中备份您的更改
    git branch your-branch-name-backup

### 应用来自develop分支的更改，在您的更改后面添加
    git rebase upstream/develop

### 切换回develop分支
    git checkout develop

### 帮助其他贡献者充分理解您所做的更改
    git commit -m "修复文档中的拼写错误"   

### 确认git将要提交的内容  
    git status       

## 冲突
可能会出现一些冲突！没关系。随时寻求帮助。如果与最新的上游`develop`分支合并导致冲突，您可以向上游仓库发起拉取请求，这样冲突合并将会公开显示。

## 最后，为了伟大的荣耀
    git push --set-upstream origin your-branch-name-backup

这是一个关于rebase的良好参考链接，以防您对技术细节非常感兴趣。[https://www.atlassian.com/git/tutorials/merging-vs-rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
