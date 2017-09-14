# 部屬到Google Engine \(GCE\)

前言:Google Cloud Platform 雲端平台有兩種服務一個叫做 Compute Engine，另一個叫做 App Engine，觀念別弄混了，以前 google 有一個專門放置 app Engine 的平台，現在已經把平台和做了整合

# 使用:

到資訊主頁後點選Compute Engine

現在有兩個月試用，以及300美元的  
免費額度

他的SSH可以直接用瀏覽器連線

# 1.基本上跟著他網頁的指示即可學會操作

創建好之後，在instance的VM執行個體下，點SSH按鈕會開啟一個視窗即可直接連線到機器

# 2.使用putty連線GCE

[https://gist.github.com/feczo/7282a6e00181fde4281b](https://gist.github.com/feczo/7282a6e00181fde4281b)

[https://cloud.google.com/compute/docs/instances/connecting-to-instance\#generatesshkeypair](https://cloud.google.com/compute/docs/instances/connecting-to-instance#generatesshkeypair)

在中繼資料下面會有一個SSH，此為你一創好instance及會出現的public key。

1.我們用putty的puttygen產生一個public key\(記得滑鼠要在puttygen下左右滑動\)，產生完後如圖

![sd](https://cloud.githubusercontent.com/assets/11001914/17313515/f3772046-588e-11e6-871c-8a2fc8f4aa4b.png)  
其中key comment的位置為你的google username

2.之後在到GCE的操控面板中  
![sdf](https://cloud.githubusercontent.com/assets/11001914/17313557/2e8ce472-588f-11e6-8adf-d3799683c7f9.png)  
點選編輯，然後新增

直接把puttygen框框中產生的public貼上到GCE面板中

\(注意:puttygen中的key comment會自動附加到public key的==後面，而GCE的使用者名稱不用填，它會自動找尋==後的字元\)

然後記得用puttygen產生private key存到電腦中

3.之後打開putty ssh=&gt;auth 把private key路徑放上

4.再到connection=&gt;data 把剛才key comment的username貼上到auto-login username欄位\(也可登入後再輸入\)

5.最後再到session的ip位置輸入你的機器外部IP即可

> PS:不建議將private key存在C曹根目錄等需要admin存取的資料夾，因為之後使用filezilla時需要存取此key會找不到檔案

# 3.其他linux使用教學

### 查看現在占用的PORT

`sudo netstat -tulpn`

### 安裝Mysql

[https://community.linuxmint.com/tutorial/view/486](https://community.linuxmint.com/tutorial/view/486)

1`sudo apt-get install apache2`

之後在外面電腦輸入你的VPS IP即可看到default畫面

接著跟著安裝一些套件

再來要改config檔案時可使用vim

輸入 `sudo vim 加檔案路徑即可`

之後記得restart `sudo /etc/init.d/apache2 restart`

最後再到網址列輸入你的ip位置後加上`/phpmyadmin`即可

### 登入MySQL prompt

`mysql -u root -p`

### 修改apache2的預設監聽port

```
1.In /etc/apache2/ports.conf, change the port as

Listen 44400

2.Then go to /etc/apache2/sites-enabled/000-default.conf

And change the first line as

<VirtualHost *:44000>

3.Now restart

sudo service apache2 restart
```

之後登入phpmyadmin改為如下  
`http://www.sakatu.com:8080/phpmyadmin/`

# 架設Node.js server無法監聽80 port

產生如下錯誤

`Error: listen EACCES 0.0.0.0:80`

必須使用sudo 加上你的node檔案執行即可

但是

這樣不安全

解法為可以用proxy server或更改ip table

參考如下文章

[http://stackoverflow.com/questions/16573668/best-practices-when-running-node-js-with-port-80-ubuntu-linode](http://stackoverflow.com/questions/16573668/best-practices-when-running-node-js-with-port-80-ubuntu-linode)

# \#開啟其他PORT

點選左上的選單然後點選網路

![](/assets/螢幕快照 2017-06-11 上午10.43.37.png)

記得`目標標記 要輸入http-server`

設定如下

0.0.0.0/0代表開放所有IP  \(記得要有/0\)

![](/assets/螢幕快照 2017-06-11 上午10.43.50.png)

# \#gcloud

VPS內的命令 可以操控VPS

如權限不夠需先login

```
gcloud auth login
```

以下為新增防火牆範例

```
gcloud compute firewall-rules create cassandra-rule --allow="tcp:9042,tcp:9160" --network="default" --description="Allow external Cassandra Thrift/CQL connections"
```



