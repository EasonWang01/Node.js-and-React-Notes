# GPG簽名

為了避免 commit 被偽造，可以使用GPG簽名。

可參考以下文章：

### GitHub

https://help.github.com/articles/signing-commits/

### GitLab

https://docs.gitlab.com/ee/user/project/repository/gpg\_signed\_commits/



----

## 如果出現failed-to-sign-the-data

於 ~/.zshrc 加上

```
export GPG_TTY=$(tty)
```

https://stackoverflow.com/questions/39494631/gpg-failed-to-sign-the-data-fatal-failed-to-write-commit-object-git-2-10-0

