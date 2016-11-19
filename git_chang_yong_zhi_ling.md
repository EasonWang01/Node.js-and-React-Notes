# Git 常用指令

###GUI
```
gitk --all
```
###刪除遠端分支
```
git push <remote name> :<branch name>

ex:
git push origin :test
```

###移除遠端的目錄(不包含本地)
```
git rm -r --cached FolderName
git commit -m "Removed folder from repository"
git push origin master
```


###有時在遠端開分之後新增檔案但和local端history不同，導致無法pull和push
可使用`git pull origin master --allow-unrelated-histories `
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