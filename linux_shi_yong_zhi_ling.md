# Unix 實用指令

# 移除包含檔案的目錄

`sudo rm -rf folderName`  
或是`Rf`

無法使用cat組合檔案內容，產生permission deny時

```
sudo bash -c 'cat 
 certificate.crt ca_bundle.crt >> bundle.crt'
```

# Debian相關筆記

```
更改網路: /etc/network/interfaces
其中需設定如下: 1.ip 2.netmask 3. gateway
之後重啟網路: /etc/init.d/networking restart

apt-get update 之 url 位置： vim  /etc/apt/sources.list

dns位置  vim /etc/resolv.conf  範例:nameserver  192.168.0.1




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

# 搜尋資料夾

```
find / -name "dir-name-here"
```

[http://www.cyberciti.biz/faq/howto-find-a-directory-linux-command/](http://www.cyberciti.biz/faq/howto-find-a-directory-linux-command/)

### 查看現在占用的PORT

```
sudo netstat -tulpn
```

OSX

```
lsof -nPi -sTCP:LISTEN
netstat -an | grep LISTEN
```

### 找出特定port的PID

```
lsof -i :port
```

\(以下為給windows使用並可關閉指定port的process \)

```
 C:\Users\username>netstat -o -n -a | findstr 0.0:3000
   TCP    0.0.0.0:3000      0.0.0.0:0              LISTENING       3116

C:\Users\username>taskkill /F /PID 3116
```

### 

### 複製資料夾並移動

記得先mkdir

```
sudo cp -R  ~/Desktop/um1215-webclient/ ./um1215-webclient
```

## 打開當前資料夾

```
open .
```

## 切換資料夾與mac不同

前面不用加`~`

```
/etc/nginx/sites-available/React-router-Redux-isomorphic-Boilerplate/dist/server
```

## 移動檔案，或是更改檔案名稱

[http://jashliao.pixnet.net/blog/post/156412841-每天一個linux指令--mv-\(用來搬移\(更名\)目錄-檔](http://jashliao.pixnet.net/blog/post/156412841-每天一個linux指令--mv-%28用來搬移%28更名%29目錄-檔)

## 上傳檔案

```
scp -i ~/Downloads/pem1.pem ~/Downloads/pem1.pem ubuntu@ec2-13-112-175-93.ap-northeast-1.compute.amazonaws.com:~/home
```

上傳資料夾\(加上-r\)

```
scp -i ~/Downloads/pem1.pem -r ./dist ubuntu@ec2-13-112-175-93.ap-northeast-1.compute.amazonaws.com:~/dist
```

## 切換當前使用者

```
 sudo su <username>
```

## 加入環境變數

#### Mac

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

> 記得是用 : 分隔

之後重啟zsh

```
 . ~/.zshrc
```

#### Linux

但Linux的環境變數路徑可能寫在~/.profile,~/.bashrc或是 ~/.bash\_profile

可參考此篇文章[http://www.yunweipai.com/archives/2305.html](http://www.yunweipai.com/archives/2305.html)

解決方法:  
1.先輸入`echo $PATH`  現在的所有路徑變數，然後先複製起來

2.輸入`~/.bashrc` 在開頭寫`export`空一格把剛才的貼上，最後加上`:`然後加上你要加上的路徑及可

E.g.

```
export PATH=/home/eason/bin:/home/eason/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/eason/Desktop/bitcoin-0.14.1/bin
```

之後Reload 設定即可

```
source ~/.bashrc
```

## 列出資料夾裡面的大容量檔案

#### Linux與macOS:

```
du -a * | sort -r -n | head -10
```

> 在mac中通常`~/Library/Caches/`會需刪除

#### Windows :

```
forfiles /S /M * /C "cmd /c if @fsize GEQ 1073741824 echo @path"
```

或是 \( 推薦 \)

```
powershell -command "$fso = new-object -com Scripting.FileSystemObject; gci -Directory | select @{l='Size'; e={$fso.GetFolder($_.FullName).Size}},FullName | sort Size -Descending | ft @{l='Size [MB]'; e={'{0:N2}    ' -f ($_.Size / 1MB)}},FullName"
```

## 找出當前資料夾的絕對路徑

```
[[ $1 = /* ]] && echo "$1" || echo "$PWD/${1#./}"
```

## 搜尋檔案

[http://www.techradar.com/how-to/computing/apple/terminal-101-using-the-find-command-1305633](http://www.techradar.com/how-to/computing/apple/terminal-101-using-the-find-command-1305633)

> `/`意思為搜尋整台電腦，也可指定路徑

```
sudo find / -name "eng.traineddata"
```

## 順序執行script

各個指令後加入`;`，如果要同時執行則是`&`

## 刪除遠端的目錄但不刪除本地

```
git rm --cached <filename>
git rm --cached -r <dir_name>
git commit -m "Removed folder from repository"
git push origin master
```

# 讓程式在背景跑

> 有時沒加sudo關掉後會斷了
>
> 或是記得執行nohub後要先ctrl+c離開然後再關閉視窗

```
sudo nohup <command> &
```

功能是接到任何離開SIGHUP訊號都不會離開  並且會產生log文件\(nohup.out\)

# OSX設定全域變數

```
vim /etc/paths
```

# 同步檔案

scp

```
scp -i private_key your_username@remotehost.edu:foobar.txt /some/local/directory
```

rsync

```
rsync --progress -r --exclude "node_modules/" --exclude  ".*/"  -e "ssh -i ~/Downloads/pem1.pem"  ubuntu@ec2-52-198-155-128.ap-northeast-1.compute.amazonaws.com:/home/ubuntu/ ./
```

> rsync可參考  [https://blog.gtwang.org/linux/rsync-local-remote-file-synchronization-commands/](https://blog.gtwang.org/linux/rsync-local-remote-file-synchronization-commands/)

#### 更改螢幕解析度

**Ｗindows**

[https://superuser.com/a/89360](https://superuser.com/a/89360)

```
下載nircmd，之後 cd 到執行檔目錄
nircmd.exe setdisplay 1920 1080 32
```

# 修改Domain對應IP

OSX:

```
sudo vim /private/etc/hosts
```

> 然後記得重整DNS cache
>
> ```
> sudo dscacheutil -flushcache
> ```

https://www.inmotionhosting.com/support/website/how-to/how-to-edit-your-hosts-file-on-a-mac

