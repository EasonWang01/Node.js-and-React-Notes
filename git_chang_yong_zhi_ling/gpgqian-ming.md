# GPG簽名

為了避免 commit 被偽造，可以使用GPG簽名。

可參考以下文章：

### GitHub

[https://help.github.com/articles/signing-commits/](https://help.github.com/articles/signing-commits/)

[https://gist.github.com/Beneboe/3183a8a9eb53439dbee07c90b344c77e](https://gist.github.com/Beneboe/3183a8a9eb53439dbee07c90b344c77e)

### GitLab

[https://docs.gitlab.com/ee/user/project/repository/gpg\_signed\_commits/](https://docs.gitlab.com/ee/user/project/repository/gpg\_signed\_commits/)

> 可以看到下圖出現 Verified 圖樣

![](<../.gitbook/assets/Screen Shot 2019-02-21 at 12.24.34 PM.png>)

## 如果一直不 prompt password

> 有時為 gpgagent 故障，可參考以下重啟

[https://stackoverflow.com/a/66927472/4622645](https://stackoverflow.com/a/66927472/4622645)&#x20;

## 如果出現failed-to-sign-the-data

於 \~/.zshrc 加上

```
export GPG_TTY=$(tty)
```

[https://stackoverflow.com/questions/39494631/gpg-failed-to-sign-the-data-fatal-failed-to-write-commit-object-git-2-10-0](https://stackoverflow.com/questions/39494631/gpg-failed-to-sign-the-data-fatal-failed-to-write-commit-object-git-2-10-0)
