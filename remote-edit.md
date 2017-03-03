#遠端寫程式

以下簡介兩種方式

# 1.使用rmate

這邊使用VSCODE開啟遠端主機上的檔案


1.安裝 Remote VScode 的vscode plugin


之後在偏好設定的使用者設定加入

```
"remote.port": 52698,

"remote.onstartup": true
```

2.在遠端主機設定config  
```
  sudo vim /etc/ssh/sshd_config => 加上 GatewayPorts yes
```
3.在VSCODE啟動server
```
按F1 之後輸入 Remote: Start server   會看到左下角有server啟動字樣
```


4.使用tunnel方式連線 

```
ssh -R 52698:127.0.0.1:52698  -i ~/Downloads/pem1.pem  ubuntu@ec2-52-198-155-128.ap-northeast-1.compute.amazonaws.com
```

5.使用rmate   安裝：https://github.com/textmate/rmate

```
rmate -p 52698 ./test1.js -f
```

即會看到遠端檔案在本機的VSCODE打開


>注意:之前使用過jmate但檔案不會正常儲存，所以建議使用rmate

----

#2.使用sync FTP
也是編輯器上的插件

在vscode可輸入`ftp-sync`

之後點選F1，然後輸入`ftp-sync init`

會在專案目錄產生一個`ftp-sync.json` 再以裡面填入相關遠端訊息

ex:

```
{
    "remotePath": "./grpcftp",
    "host": "ec2-52-198-155-128.ap-northeast-1.compute.amazonaws.com",
    "username": "ubuntu",
    "password": "password",
    "port": 22,
    "secure": false,
    "protocol": "sftp",
    "uploadOnSave": true,
    "passive": false,
    "debug": false,
    "privateKeyPath": "/Users/eason.wang/Downloads/pem1.pem",
    "passphrase": null,
    "ignore": [
        "\\.vscode",
        "\\.git",
        "\\.DS_Store"
    ]
}
```

之後儲存檔案後隨便開一個檔案修改後儲存，左下方會顯示uploading，之後遠端主機即會出現一個和設定檔中`remotePath`相同的資料夾，裡面即有剛才修改的檔案

>注意:
1.privateKeyPath要是絕對路徑，預設config是FTP，建議改為SFPT與22port

>2.可在private key所在資料夾輸入 [[ $1 = /* ]] && echo "$1" || echo "$PWD/${1#./}" 即會顯示絕對路徑

>3.如果是用密碼登入不是用private則可以不用填privateKeyPath，而是要填password