# 部屬到Google Engine (GCE)

到資訊主頁後點選Compute Engine

現在有兩個月試用，以及300美元的
免費額度

他的SSH可以直接用瀏覽器連線

基本上跟著他網頁的指示即可學會操作

在instance的VM執行個體下，點SSH按鈕會開啟一個視窗即可直接連線到機器

#使用putty連線GCE

https://gist.github.com/feczo/7282a6e00181fde4281b

在中繼資料下面會有一個SSH，此為你一創好instance及會出現的public key。

我們用putty的puttygen產生一個public key(記得滑鼠要在puttygen下左右滑動)，產生完後如圖

![sd](https://cloud.githubusercontent.com/assets/11001914/17313515/f3772046-588e-11e6-871c-8a2fc8f4aa4b.png)
其中key comment的位置為你的google username

之後在到GCE的操控面板中
![sdf](https://cloud.githubusercontent.com/assets/11001914/17313557/2e8ce472-588f-11e6-8adf-d3799683c7f9.png)
點選編輯，然後新增

直接把puttygen框框中產生的public貼上到GCE面板中

(注意:puttygen中的key comment會自動附加到public key的==後面，而GCE的使用者名稱不用填，它會自動找尋==後的字元)




##查看現在占用的PORT

`sudo netstat -tulpn`

#安裝Mysql
https://community.linuxmint.com/tutorial/view/486

1`sudo apt-get install apache2`

之後在外面電腦輸入你的VPS IP即可看到default畫面

接著跟著安裝一些套件

再來要改config檔案時可使用vim

輸入 `sudo vim 加檔案路徑即可打開檔案編輯`

之後記得restart `sudo /etc/init.d/apache2 restart`

最後再到網址列輸入你的ip位置後加上`/phpmyadmin`即可

####安全性問題

不建議production還放著phpmyadmin，而mysql預設是只能用localhost登入，但安裝phpmyadmin後如未特別設定則任何主機均可修改資料庫

http://serverfault.com/questions/68264/would-you-install-phpmyadmin-on-a-production-web-server

https://www.google.com.tw/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=mysql+only+allow+localhost


##更改網域名稱DNS


參考此文章跟影片

https://www.youtube.com/watch?v=8l--TJabo2M

http://howtodosteps.blogspot.tw/2015/08/how-to-assigntransfer-godaddy-domain.html

下面以godaddy為範例，在我的網域=>管理DNS  下面的域名伺服器新增如下(先到GCE查看)如圖

![1](https://cloud.githubusercontent.com/assets/11001914/17292905/68992eca-581f-11e6-9612-806d5f78b0be.png)

![2](https://cloud.githubusercontent.com/assets/11001914/17292906/6a0ca958-581f-11e6-8fce-af92d16997f3.png)

設定GCE
![dns3](https://cloud.githubusercontent.com/assets/11001914/17297579/df656d0c-5838-11e6-8437-f9cd7a01546f.png)


之後等待最長48小時。(這兩種搭配個人測試是一分鐘內生效的)

