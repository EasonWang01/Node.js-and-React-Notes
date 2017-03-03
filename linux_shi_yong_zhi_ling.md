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

##移動檔案，或是更改檔案名稱


http://jashliao.pixnet.net/blog/post/156412841-%E6%AF%8F%E5%A4%A9%E4%B8%80%E5%80%8Blinux%E6%8C%87%E4%BB%A4--mv-(%E7%94%A8%E4%BE%86%E6%90%AC%E7%A7%BB(%E6%9B%B4%E5%90%8D)%E7%9B%AE%E9%8C%84-%E6%AA%94

##上傳檔案

```
scp -i ~/Downloads/pem1.pem ~/Downloads/pem1.pem ubuntu@ec2-13-112-175-93.ap-northeast-1.compute.amazonaws.com:~/home
```

上傳資料夾(加上-r)

```

scp -i ~/Downloads/pem1.pem -r ./dist ubuntu@ec2-13-112-175-93.ap-northeast-1.compute.amazonaws.com:~/dist

```

##切換當前使用者

```
 sudo su <username>
```

##加入環境變數

假設原本要指定完整路徑才可執行檔案，可直接把他加入環境變數的PATH中

```
export PATH=/Volumes/FFmpeg\ 83544-g965f35b:$PATH

```

但這樣會在重啟terminal後變數就消失，並且把原本變數覆蓋，解決方法為加入bashrc

如使用zsh則是`~/.zshrc`

寫入path如下
```
export PATH=/usr/local/bin:/usr/local/sbin/:/usr/bin:/bin:/usr/sbin:/sbin:/usr/X11/bin:/Volumes/FFmpeg\ 83544-g965f35b

```
之後重啟zsh

```
 . ~/.zshrc
```

##列出資料夾裡面的大容量檔案


```
du -a * | sort -r -n | head -10
```

##找出當前資料夾的絕對路徑

```
[[ $1 = /* ]] && echo "$1" || echo "$PWD/${1#./}"
```