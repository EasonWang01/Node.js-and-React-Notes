# 部署到AWS

```
記得開security group 來開port
```

https://www.youtube.com/watch?v=WxhFq64FQzA

##上傳檔案

如果沒辦法用SFTP出現permission deny須先把該EC2上的資料夾使用`sudo chmod 777 資料夾名稱 `來開啟權限即可

GUI部分，在`window`可用WINSCP或FileZillz在`Mac`可用CyberDuck

http://stackoverflow.com/questions/20939562/scp-permission-denied-publickey-on-ec2-only-when-using-r-flag-on-directories

##注意事項
如果上傳檔案時沒有權限，但你覺得一個一個資料夾開權限太麻煩，於是在usr之類的大資料夾整個開權限後會出現整個sudo出現錯誤

```
sudo: /usr/bin/sudo must be owned by uid 0 and have the setuid bit set
```
目前還沒找到解法，網路上目前推薦重灌系統