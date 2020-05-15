# Git 實用指令

## Git 實用指令

#### GUI

```text
gitk --all
```

or

```text
npm install ungit -g
```

#### 查看特定檔案的修改紀錄

```text
gitk --follow [filename]
```

#### 取消git add .

```text
git reset
```

#### 刪除遠端分支

> 記得該分支不可是Default branch

```text
git push origin :the_remote_branch
或是
git push origin --delete the_remote_branch
```

#### 移除本地分支

```text
git branch -D <分支名稱>
```

#### 移除遠端的目錄\(不包含本地\)

```text
git rm -r --cached FolderName
git commit -m "Removed folder from repository"
git push origin master
```

#### 將一個分支的所有內容推送到另一分支

```text
git push origin <原分枝>:<新增之分枝>
```

#### 有時在遠端開分之後新增檔案但和local端history不同，導致無法pull和push

可使用`git pull origin master --allow-unrelated-histories`

```text
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

### 移除遠端上一次的push

\(但記得這樣會直接退回到你上次的HEAD^ 如果你還沒pull則會把別人的push一起移除\)

```text
git push -f origin HEAD^:master
```

或是使用以下，然後把commit註解掉

```text
git rebase -i HEAD~2
```

> `git rebase origin/master`will merge in the requested branch `origin/master`and apply the commits that you have made locally to the top of the history without creating a merge commit.

### 取消上次commit且維持檔案內容

```text
git reset HEAD~
```

[http://stackoverflow.com/questions/927358/how-to-undo-last-commits-in-git](http://stackoverflow.com/questions/927358/how-to-undo-last-commits-in-git)

### 回到上次commit的檔案狀態，刪除檔案內容

```text
git reset --hard HEAD~1
```

### 回到上次commit檔案狀態，刪除檔案內容，但把檔案內容先儲存

```text
git stash
git stash list //列出曾經stash過的東西
git stash pop //把最近的stash套用 並從list刪除
git stash clear // 清空stash list
```

說明：你會發現跟reset hard類似，但他會把你的改變先儲存，之後可用`$ git stash apply`回復

[https://git-scm.com/book/zh-tw/v1/Git-工具-儲藏-Stashing](https://git-scm.com/book/zh-tw/v1/Git-工具-儲藏-Stashing)

> 有關Bitbucket因為會在url產生一個隨機hash，所以每次push上去後重新整理看不到更新，必須重新進入repo一次才看得到更新，但github重新整理即可看到

### 把別人遠端新開的分支fetch到你的電腦

```text
git fetch
git checkout 分支名稱
```

## 使用 Git rebase 讓 Git Pull 時避免auto merge commit

> 時常使用git pull之後會自動出現Merge branch 'master' of....之類的訊息在自己的commit之前
>
> 只要使用git pull --rebase即可解決

```text
git:(feature/branch) git pull --rebase origin master
git:(feature/branch) 修改衝突
git:(feature/branch) git add .
git:(feature/branch) git rebase --continue
git:(feature/branch) git push origin <feature/branch> -f
```

[http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/](http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/)

[https://ihower.tw/blog/archives/3843](https://ihower.tw/blog/archives/3843)

## 使用 Git rebase 來 squash commit

```text
git rebase -i <feature 第一個commit之前的前一個 commit>
```

當合併分枝時想乾淨一點，可以用squash

```text
pick 2bab3e7 test1
squash 27f6ed6 test2
// 例如此例 27f6ed6 會合併進去 2bab3e7
```

記得第一個pick 要放在該feature 的前一個commit。

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/螢幕快照%202020-02-13%20上午10.56.00.png)

> squash 可改為 fixup 可以移除commit訊息

## 把最新的master內容更新到feature branch

```javascript
git checkout <feature branch>
git pull origin master --rebase
// 如果有conflict，修完conflict後git add，然後git rebase --continue
```

## 有關submodule

[https://blog.wu-boy.com/2011/09/introduction-to-git-submodule/](https://blog.wu-boy.com/2011/09/introduction-to-git-submodule/)

[http://blog.johnsonlu.org/git-submodule/](http://blog.johnsonlu.org/git-submodule/)

[http://blog.chh.tw/posts/git-submodule/](http://blog.chh.tw/posts/git-submodule/)

主要是引用第三方的git專案，git clone時這些submodule 需要--recursive才會一起下載

或是可以之後輸入

```text
 git submodule init
 git submodule update
```

## 修改上一次commit 的說明

```text
git commit --amend
```

## 修改以前多次commit 說明

[https://help.github.com/articles/changing-a-commit-message/](https://help.github.com/articles/changing-a-commit-message/)

[https://git-scm.com/book/zh-tw/v1/Git-工具-重寫歷史](https://git-scm.com/book/zh-tw/v1/Git-工具-重寫歷史)

```text
git rebase -i HEAD~3
```

```text
//之後會進入vim如下

pick e499d89 Delete CNAME
reword 0c39034 Better README
reword f7fde4a Change the commit message but push the same commit.

//把要修改的前面從pick改為reword即可
```

## 移除以前的commit

```text
git rebase -i HEAD~4
```

```text
之後會產生列表，把你不要的移除或註解掉即可，如下。
```

```text
pick 2f05aba ... #will be preserved
#pick 3371cec ... #will be deleted
#pick daed25c ... #will be deleted
pick e2b2a84 ... #will be preserved
```

[https://stackoverflow.com/questions/9725156/remove-old-git-commits](https://stackoverflow.com/questions/9725156/remove-old-git-commits)

> 如果要更改遠端Repo必須要Force Push
>
> 如果要移除最後一次commit 則至少要到HEAD~2 `git rebase -i HEAD~2` 然後把最後一次commit註解掉

## 重置歷史commit

[https://stackoverflow.com/a/32765827](https://stackoverflow.com/a/32765827)

先checkout新的orphan branch

```text
git checkout --orphan mainsource01
```

然後移除舊的branch

```text
git push origin --delete mainsource -f
```

## 移除所有Commit 紀錄

照著下面的步驟輸入即可。

會把原先master分支刪除。

[https://stackoverflow.com/a/26000395/8606842](https://stackoverflow.com/a/26000395/8606842)

## Git Reset

```text
// 把之前的commit紀錄刪除 並且倒回為之前檔案狀態

git reset --hard HEAD^  //HEAD^指的是前一次  HEAD^^前兩次  以此類推

git reset --hard ORIG_HEAD  //返回reset前的內容
```

## Git cherry-pick

把以前的commit檔案內容加入到現在的內容

```text
git cherry-pick <sha1 hash>
```

一次pick多個commit

[https://stackoverflow.com/questions/1994463/how-to-cherry-pick-a-range-of-commits-and-merge-into-another-branch/1994491\#1994491](https://stackoverflow.com/questions/1994463/how-to-cherry-pick-a-range-of-commits-and-merge-into-another-branch/1994491#1994491)

## Git revert

可以返回上一次的提交 並且需要commit一次

```text
git revert HEAD //返回至上一次的提交  並且commit
```

revert 用來取消以前的Commit，取消後會產生出新的commit，可參考 :

[http://rubyist.marsz.tw/blog/2012-01-17/git-reset-and-revert-to-rollback-commit/](http://rubyist.marsz.tw/blog/2012-01-17/git-reset-and-revert-to-rollback-commit/)

## Git Rebase

讓merge後 ，圖表不顯示分支紀錄，並且可用來移除以前的Commit。

```text
// 加上 -i 為進入互動式模式

git rebase master
git rebase --continue
```

## Git Blame ./filename

> 查看每行code是誰commit的

## Git format-patch\(打包以前的commit\)

```text
因為merge以前的commit通常會認為歷史一樣 所以要用別的方法
可用cherry-pick或是format-patch打包舊的commit再apply(am)
```

[https://stackoverflow.com/questions/46638741/git-add-code-from-old-commit](https://stackoverflow.com/questions/46638741/git-add-code-from-old-commit)

## Git shallow glone

只把特定深度數字內的commit一起clone到本地

```text
使用git clone --depth <深度 num> <url>
```

[https://www.perforce.com/blog/git-beyond-basics-using-shallow-clones](https://www.perforce.com/blog/git-beyond-basics-using-shallow-clones)

## 假設最新的不再master branch 在其他HEAD\(ex: bc50ed4\)但想把master更新為bc50ed4

[https://stackoverflow.com/questions/2763006/make-the-current-git-branch-a-master-branch](https://stackoverflow.com/questions/2763006/make-the-current-git-branch-a-master-branch)

```text
git checkout master

git reset --hard bc50ed4
// 把master資料替換為HEAD bc50ed4

git push -f origin master
```

## .gitignore沒反應

如果在加入.gitignore檔案之前已經提交過檔案了，則git 會有cache造成寫了.gitignore依然會提交該檔案

可使用以下指令

```text
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```

\(但這樣會在遠端刪除該cached檔案\)

[https://stackoverflow.com/questions/11451535/gitignore-is-not-working](https://stackoverflow.com/questions/11451535/gitignore-is-not-working)

## 查看目前修改過但還沒add的檔案

```text
git diff --stat
```

## 查看目前已add 但還沒commit的檔案

```text
git diff --cached --name-only
```

[https://stackoverflow.com/questions/2298047/git-ls-files-howto-identify-new-files-added-not-committed](https://stackoverflow.com/questions/2298047/git-ls-files-howto-identify-new-files-added-not-committed)

[https://git-scm.com/docs/git-diff](https://git-scm.com/docs/git-diff)

## 查看目前已commit還沒push的檔案

```text
 git show –name-only {commit hash}
```

## 取消Rebase

如果Rebase完了要Merge，不讓你checkout到原本的branch，可以使用如下

```text
git reset --merge
```

然後再`git checkout ...` 即可

## Checkout 先前 commit 後 merge

```text
git checkout a59867a8b0
```

> 如果有時想回到先前的commit 內容，可以checkout 回以前的 commit hash，但如果之後又在此commit上改東西後，想讓當前內容變為最新的內容的話可以用如下

```text
git checkout <Branch name>
git merge a59867a8b0
```

## 回復以前commit的內容，並覆蓋develop

```text
git branch -f develop <git commit hash>
```

## 一次解決所有 conflict

全部檔案套用遠端的

```text
grep -lr '<<<<<<<' . | xargs git checkout --theirs
```

全部檔案套用本地的

```text
grep -lr '<<<<<<<' . | xargs git checkout --ours
```

[https://easyengine.io/tutorials/git/git-resolve-merge-conflicts](https://easyengine.io/tutorials/git/git-resolve-merge-conflicts)

## 快速更改上次commit的作者

```text
git commit --amend --author="yicheng.wang <yicheng.wang@ccc.com>"
git push origin master --force
```

> 如果要更改多次的用rebase
>
> [https://www.git-tower.com/learn/git/faq/change-author-name-email](https://www.git-tower.com/learn/git/faq/change-author-name-email)

## 直接給予權限

```text
git remote set-url origin https://<username>:<password>@github.com/....git
```

## 改大小寫後push確沒改到

[https://stackoverflow.com/questions/10523849/changing-capitalization-of-filenames-in-git/16071375](https://stackoverflow.com/questions/10523849/changing-capitalization-of-filenames-in-git/16071375)

改完後要記得用以下方法改git內的暫存檔名，會自動stage

```text
git mv --force Myclass.java MyClass.java
```

## 把某個pull request直接加到local

```text
git pull origin pull/<request號碼>/head
```

> request號碼為7 e.g.：[https://github.com/test/react-examination/pull/7](https://github.com/bridge5/react-examination/pull/7)

## Merge時省略特定檔案

[https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes\#\_merge\_strategies](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes#_merge_strategies)

輸入

```text
git config --global merge.ours.driver true
```

之後 `vim $HOME/.gitconfig` 會顯示如下

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/螢幕快照%202019-08-13%20下午4.07.31.png)

然後於專案內輸入`vim .gitattributes`

新增以下內容

```text
<要省略的檔案名稱> merge=ours
```

之後commit 在 merge 即可

## 創建空的branch

```text
git checkout --orphan <分支名稱>
git rm --cached -r .
```

> 創建新的branch原本檔案內容會帶過來，需要主動 discard

## GIt commit 後只有一個檔案

裡面只有hash

這個問題原因是多個資料夾內有sub folder，然後你進去sub folder 做了`rm -rf .git`

這時只有先單獨commit subfolder內的單個檔案然後 force push，之後才commit 其他的即可

## Git 自訂快捷鍵

> 把以下加在~/.zshrc 內 之後可以用 gac ... 直接取代 `git add . && git commit -m '....'`

```text
gac() {
    string=""
    for i in "$@"; do string+=" $i"; done;
    git add .
    git commit -m "$string"
}
```

## 其他不錯文章

[http://www.techug.com/post/10-tips-git-next-level.html](http://www.techug.com/post/10-tips-git-next-level.html)

