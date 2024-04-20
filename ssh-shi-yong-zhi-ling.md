---
description: 食用指令
---

# SSH 實用指令

## SSH Agent Forwarding

讓 A, B 兩台遠端主機互相傳送檔案

1.  **w將兩台主機的私鑰都加入本機 SSH agent:**

    ```bash
    ssh-add ~/downloads/geth-node-1.pem
    ssh-add ~/downloads/geth-node-2.pem
    ```
2.  **SSH  EC2 instance with agent forwarding:**

    ```bash
    bashCopy codessh -A -i "~/downloads/geth-node-1.pem" ubuntu@ec2-47-128-199-122.ap-southeast-1.compute.amazonaws.com
    ```
3.  **From the first instance, attempt to SCP the file using agent forwarding to authenticate on the second instance:**

    ```bash
    bashCopy codescp ~/eth-pos-devnet/devnet/genesis.ssz ubuntu@ec2-18-136-241-109.ap-southeast-1.compute
    ```
