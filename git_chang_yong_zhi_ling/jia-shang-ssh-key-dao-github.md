# 加上 SSH-key 到 GitHub

> 使用 remote repo時如果用 ssh 方法進行 clone, fetch ,pull, push時會需要放置電腦本地金鑰到 Github 上提供驗證。

## 產生金鑰

輸入以下：

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

之後輸入金鑰密碼（也可不輸入）。

## 新增其他Key

> 有時如果電腦已經存在金鑰時，加入 GitHub 相同名稱金鑰會產生錯誤，可取名不同金鑰名稱。

1.到此文件：`~/.ssh/config`

2.新增如下：

```
Host github.com
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa_github
```

3.將私鑰加入 ssh-agent 與 keychain

```
ssh-add -K ~/.ssh/id_rsa_github
```

參考文章：

[https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

[https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

