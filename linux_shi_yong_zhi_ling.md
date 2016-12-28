# Unix 實用指令

#移除包含檔案的目錄

`sudo rm -rf folderName`
或是`Rf`

無法使用cat組合檔案內容，產生permission deny時
```
sudo bash -c 'cat 
 certificate.crt ca_bundle.crt >> bundle.crt'
```



#Debian相關筆記
```

更改網路:/etc/network/interfaces
其中需設定如下:1.ip 2.netmask 3. gateway
之後重啟網路:/etc/init.d/networking restart

debian的apt-get update   url位置 vim  /etc/apt/sources.list

網路的router位置  vim /etc/resolv.conf  範例:nameserver  192.168.0.1




apt-get update後出現public key問題

使用如下指令加入key 

($key為其error顯示的key號碼)
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys $key

如果出現一些lib版本不符合則輸入  apt-get -f install

目前nginx無法安裝  但可安裝apache2  (apt-get apache2)


使用git:

直接輸入apt-get install git

之後git clone時可能會出現不信任憑證的錯誤
輸入以下即可
git config --global http.sslverify false

之後git clone可正常執行 
```

#搜尋資料夾

```
find / -name "dir-name-here"
```
http://www.cyberciti.biz/faq/howto-find-a-directory-linux-command/

###查看現在占用的PORT

```
sudo netstat -tulpn
```

OSX

```
sudo lsof -nPi -sTCP:LISTEN
```

###找出特定port的PID

```
lsof -i :port
```

###複製資料夾並移動

記得先mkdir

```
sudo cp -R  ~/Desktop/um1215-webclient/ ./um1215-webclient
```

##打開當前資料夾

```
open .
```


##切換資料夾與mac不同

前面不用加`~`

```
/etc/nginx/sites-available/React-router-Redux-isomorphic-Boilerplate/dist/server
```