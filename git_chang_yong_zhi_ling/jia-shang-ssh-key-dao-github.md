# 加上 SSH-key 到 GitHub

## 加上 SSH-key 到 GitHub

> 使用 remote repo時如果用 ssh 方法進行 clone, fetch ,pull, push時會需要放置電腦本地金鑰到 Github 上提供驗證。

### 1.產生金鑰

輸入以下：

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

之後輸入金鑰密碼（也可不輸入）。

### 2.新增其他Key

> 有時如果電腦已經存在金鑰時，加入 GitHub 相同名稱金鑰會產生錯誤，可取名不同金鑰名稱。

1.到此文件：`~/.ssh/config`

2.新增如下：

```
Host github.com
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa_github
```

> 第二部驟不一定要做

3.將私鑰加入 ssh-agent 與 keychain

```
ssh-add -K ~/.ssh/id_rsa_github
```

參考文章：

[https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

[https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

### 3.複製電腦的公鑰到GitHub

```
pbcopy < ~/.ssh/公鑰名稱.pub
```

然後貼到GitHub

![](https://github.com/easonwang01/web\_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%202018-10-27%20%E4%B8%8A%E5%8D%8810.57.08.png)

## Windows 產生金鑰

產生金鑰

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

```
start-ssh-agent.cmd
ssh-add ~/.ssh/id_rsa
```

加到github

```
clip < ~/.ssh/id_rsa
然後 https://github.com/settings/keys 貼上
```

## 讓不同Github帳號有不同KEY

```
# Default github account: test1
Host github.com
   HostName github.com
   IdentityFile ~/.ssh/test1_private_key
   IdentitiesOnly yes
   
# Other github account: test2
Host github-test2
   HostName github.com
   IdentityFile ~/.ssh/test2_private_key
   IdentitiesOnly yes
```

[https://gist.github.com/oanhnn/80a89405ab9023894df7](https://gist.github.com/oanhnn/80a89405ab9023894df7)

[https://stackoverflow.com/a/10056098](https://stackoverflow.com/a/10056098)

## GitHub Deploy Key 教學

當有多個 repo 需要 clone 到同個 server時，因為 deploy key 不能重複，所以每個 github repo 放入的 deploy key 要不同，所以要在 server 產生多個公私鑰然後取不同名稱，放入 repo，然後在 server 上設置 ssh config

![](<../.gitbook/assets/截圖 2023-03-20 下午3.01.29.png>)

{% embed url="https://docs.github.com/en/developers/overview/managing-deploy-keys#using-multiple-repositories-on-one-server" %}

SSH config 範例

```
Host github.com-repo1
        Hostname github.com
        IdentityFile=~/.ssh/id_rsa
Host github.com-repo2
        Hostname github.com
        IdentityFile=~/.ssh/ui-deploy-key
```

這邊要注意， git url 要改成如下，不可直接複製 github 上的 git url

> 以 repo 名稱為 repo-1， github account 名稱為 user1 為例

```
git clone git@github.com-repo-1:user1/repo-1.git
```
