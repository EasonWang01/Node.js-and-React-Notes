# Git 實用指令

### GUI

```
gitk --all
```

or

```
npm install ungit -g
```

### 查看特定檔案的修改紀錄

```
gitk --follow [filename]
```

### 取消git add .

```
git reset
```

### 刪除遠端分支

```
git push <remote name> :<branch name>

ex:
git push origin :test
```

### 移除遠端的目錄\(不包含本地\)

```
git rm -r --cached FolderName
git commit -m "Removed folder from repository"
git push origin master
```

### 有時在遠端開分之後新增檔案但和local端history不同，導致無法pull和push

可使用`git pull origin master --allow-unrelated-histories`

```
$git pull

fatal: refusing to merge unrelated histories

$git push origin master

To https://github.com/EasonWang01/React-Editor.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/EasonWang01/React-Editor.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.


$git pull origin master --allow-unrelated-histories 

ok
```

## 移除遠端上一次的push

\(但記得這樣會直接退回到你上次的HEAD^ 如果你還沒pull則會把別人的push一起移除\)

```
git push -f origin HEAD^:master
```

## 取消上次commit且維持檔案內容

```
git reset HEAD~
```

[http://stackoverflow.com/questions/927358/how-to-undo-last-commits-in-git](http://stackoverflow.com/questions/927358/how-to-undo-last-commits-in-git)

## 回到上次commit的檔案狀態，刪除檔案內容

```
git reset --hard HEAD~1
```

## 回到上次commit檔案狀態，刪除檔案內容，但把檔案內容先儲存

```
git stash
git stash list //列出曾經stash過的東西
git stash pop //把最近的stash套用 並從list刪除
git stash clear // 清空stash list
```

說明：你會發現跟reset hard類似，但他會把你的改變先儲存，之後可用`$ git stash apply`回復

[https://git-scm.com/book/zh-tw/v1/Git-工具-儲藏-Stashing](https://git-scm.com/book/zh-tw/v1/Git-工具-儲藏-Stashing)

> 有關Bitbucket因為會在url產生一個隨機hash，所以每次push上去後重新整理看不到更新，必須重新進入repo一次才看得到更新，但github重新整理即可看到

## 把別人遠端新開的分支fetch到你的電腦

```
git fetch
git checkout 分支名稱
```

# Git Pull 時避免auto merge commit

> 時常使用git pull之後會自動出現Merge branch 'master' of....之類的訊息在自己的commit之前
>
> 只要使用git pull --rebase即可解決

[http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/](http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/)

[https://ihower.tw/blog/archives/3843](https://ihower.tw/blog/archives/3843)

# 有關submodule

[https://blog.wu-boy.com/2011/09/introduction-to-git-submodule/](https://blog.wu-boy.com/2011/09/introduction-to-git-submodule/)

[http://blog.johnsonlu.org/git-submodule/](http://blog.johnsonlu.org/git-submodule/)

[http://blog.chh.tw/posts/git-submodule/](http://blog.chh.tw/posts/git-submodule/)

主要是引用第三方的git專案，git clone時這些submodule 需要--recursive才會一起下載

或是可以之後輸入

```
 git submodule init
 git submodule update
```

# \#修改上一次commit 的說明

```
git commit --amend
```

# \# 修改以前多次commit 說明

[https://help.github.com/articles/changing-a-commit-message/](https://help.github.com/articles/changing-a-commit-message/)

[https://git-scm.com/book/zh-tw/v1/Git-工具-重寫歷史](https://git-scm.com/book/zh-tw/v1/Git-工具-重寫歷史)

```
git rebase -i HEAD~3
```

```
//之後會進入vim如下

pick e499d89 Delete CNAME
reword 0c39034 Better README
reword f7fde4a Change the commit message but push the same commit.

//把要修改的前面從pick改為reword即可
```

# Git Reset

```
// 把之前的commit紀錄刪除 並且倒回為之前檔案狀態

git reset --hard HEAD^  //HEAD^指的是前一次  HEAD^^前兩次  以此類推

git reset --hard ORIG_HEAD  //返回reset前的內容
```

# Git cherry-pick

把以前的commit檔案內容加入到現在的內容

```
git cherry-pick <sha1 hash>
```

一次pick多個commit

[https://stackoverflow.com/questions/1994463/how-to-cherry-pick-a-range-of-commits-and-merge-into-another-branch/1994491\#1994491](https://stackoverflow.com/questions/1994463/how-to-cherry-pick-a-range-of-commits-and-merge-into-another-branch/1994491#1994491)

# Git revert

可以返回上一次的提交 並且需要commit一次

```
git revert HEAD //返回至上一次的提交  並且commit
```

# Git Rebase

```
讓merge之後   圖表不顯示分支紀錄

// 加上 -i 為進入互動式模式

git rebase master
git rebase --continue
```

# Git Blame ./filename

> 查看每行code是誰commit的

# Git format-patch\(打包以前的commit\)

```
因為merge以前的commit通常會認為歷史一樣 所以要用別的方法
可用cherry-pick或是format-patch打包舊的commit再apply(am)
```

[https://stackoverflow.com/questions/46638741/git-add-code-from-old-commit](https://stackoverflow.com/questions/46638741/git-add-code-from-old-commit)

# Git shallow glone

只把特定深度數字內的commit一起clone到本地

```
使用git clone --depth <深度 num> <url>
```

[https://www.perforce.com/blog/git-beyond-basics-using-shallow-clones](https://www.perforce.com/blog/git-beyond-basics-using-shallow-clones)

# 假設最新的不再master branch 在其他HEAD\(ex: bc50ed4\)但想把master更新為bc50ed4

[https://stackoverflow.com/questions/2763006/make-the-current-git-branch-a-master-branch](https://stackoverflow.com/questions/2763006/make-the-current-git-branch-a-master-branch)

```
git checkout master

git reset --hard bc50ed4
// 把master資料替換為HEAD bc50ed4

git push -f origin master
```

# .gitignore沒反應

如果在加入.gitignore檔案之前已經提交過檔案了，則git 會有cache造成寫了.gitignore依然會提交該檔案

可使用以下指令

```
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```

\(但這樣會在遠端刪除該cached檔案\)

[https://stackoverflow.com/questions/11451535/gitignore-is-not-working](https://stackoverflow.com/questions/11451535/gitignore-is-not-working)

# 查看目前修改過但還沒add的檔案

```
git ls-files --others --exclude-standard
```

# 查看目前已add 但還沒commit的檔案

```
git diff --cached --name-only
```

https://stackoverflow.com/questions/2298047/git-ls-files-howto-identify-new-files-added-not-committed

https://git-scm.com/docs/git-diff

# 查看目前已commit還沒push的檔案

```
 git show –name-only {commit hash}
```

# 其他不錯文章

[http://www.techug.com/post/10-tips-git-next-level.html](http://www.techug.com/post/10-tips-git-next-level.html)

