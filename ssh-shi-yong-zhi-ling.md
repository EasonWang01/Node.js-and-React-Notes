# SSH 實用指令

## SSH Agent Forwarding

> 讓 A, B 兩台遠端主機互相傳送檔案

1.  **將兩台主機的私鑰都加入本機 SSH agent:**

    ```bash
    ssh-add ~/downloads/geth-node-1.pem
    ssh-add ~/downloads/geth-node-2.pem
    ```
2.  **SSH 進入第一台  EC2 instance with agent forwarding:**

    ```bash
    ssh -A -i "~/downloads/geth-node-1.pem" ubuntu@ec2-....ap-southeast-1.compute.amazonaws.com
    ```
3.  **From the first instance, attempt to SCP the file using agent forwarding to authenticate on the second instance:**

    ```bash
    scp ~/target/file ubuntu@ec2-....ap-southeast-1.compute:/path/to/destination/
    ```



## SSH Tunnel

讓本地端可以透過 EC2 連線到 AWS RDS (EC2 要先與 RDS 設定好 security group 可互相連線)

```
 ssh -i "~/ec2-ssh-key.pem" -N -L localhost:5432:RDS..southeast-1.rds.amazonaws.com:5432  ubuntu@ec2ip.ap-southeast-1.compute.amazonaws.com
```

## 將 SSH key 加入 SSH Agent&#x20;

```
ssh-add -K ~/.ssh/...keyname
```

## 檢查目前讀取在 SSH Agent 的 keys

```
ssh-add -l
```

## 移除所有 SSH Agent keys

> delete all the private keys stored in the SSH agent

```
ssh-add -D 
```

## 設置 git push 嘗試多個  ssh key

> 開啟：vim \~/.ssh/config

```
Host bitbucket.org
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/key1
  IdentityFile ~/.ssh/key2
  IdentityFile ~/.ssh/key3
```
