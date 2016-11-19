# Git 常用指令

GUI
```
gitk --all
```
刪除遠端分支
```
git push <remote name> :<branch name>

ex:
git push origin :test
```

移除遠端的目錄(不包含本地)
```
git rm -r --cached FolderName
git commit -m "Removed folder from repository"
git push origin master
```

