遠端寫程式

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