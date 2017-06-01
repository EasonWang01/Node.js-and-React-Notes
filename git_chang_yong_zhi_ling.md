# Git 實用指令

### GUI

```
gitk --all
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
```

說明：你會發現跟reset hard類似，但他會把你的改變先儲存，之後可用`$ git stash apply`回復

[https://git-scm.com/book/zh-tw/v1/Git-工具-儲藏-Stashing](https://git-scm.com/book/zh-tw/v1/Git-工具-儲藏-Stashing)

> 有關Bitbucket因為會在url產生一個隨機hash，所以每次push上去後重新整理看不到更新，必須重新進入repo一次才看得到更新，但github重新整理即可看到

## 把別人遠端新開的分支fetch到你的電腦

```
git fetch
git checkout 分支名稱
```

# 有關submodule

[http://blog.johnsonlu.org/git-submodule/](http://blog.johnsonlu.org/git-submodule/)

http://blog.chh.tw/posts/git-submodule/

主要是引用第三方的git專案，git clone時這些submodule 需要--recursive才會一起下載

