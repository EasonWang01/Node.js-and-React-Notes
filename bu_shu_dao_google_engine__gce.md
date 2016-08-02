# 部屬到Google Engine (GCE)

到資訊主頁後點選Compute Engine

現在有兩個月試用，以及300美元的
免費額度

他的SSH可以直接用瀏覽器連線

#1.基本上跟著他網頁的指示即可學會操作

在instance的VM執行個體下，點SSH按鈕會開啟一個視窗即可直接連線到機器

#2.使用putty連線GCE

https://gist.github.com/feczo/7282a6e00181fde4281b

https://cloud.google.com/compute/docs/instances/connecting-to-instance#generatesshkeypair

在中繼資料下面會有一個SSH，此為你一創好instance及會出現的public key。

1.我們用putty的puttygen產生一個public key(記得滑鼠要在puttygen下左右滑動)，產生完後如圖

![sd](https://cloud.githubusercontent.com/assets/11001914/17313515/f3772046-588e-11e6-871c-8a2fc8f4aa4b.png)
其中key comment的位置為你的google username

2.之後在到GCE的操控面板中
![sdf](https://cloud.githubusercontent.com/assets/11001914/17313557/2e8ce472-588f-11e6-8adf-d3799683c7f9.png)
點選編輯，然後新增

直接把puttygen框框中產生的public貼上到GCE面板中

(注意:puttygen中的key comment會自動附加到public key的==後面，而GCE的使用者名稱不用填，它會自動找尋==後的字元)

然後記得用puttygen產生private key存到電腦中

3.之後打開putty ssh=>auth 把private key路徑放上

4.再到connection=>data 把剛才key comment的username貼上到auto-login username欄位(也可登入後再輸入)

5.最後再到session的ip位置輸入你的機器外部IP即可


#3.其他linux使用教學

###查看現在占用的PORT

`sudo netstat -tulpn`

###安裝Mysql
https://community.linuxmint.com/tutorial/view/486

1`sudo apt-get install apache2`

之後在外面電腦輸入你的VPS IP即可看到default畫面

接著跟著安裝一些套件

再來要改config檔案時可使用vim

輸入 `sudo vim 加檔案路徑即可`

之後記得restart `sudo /etc/init.d/apache2 restart`

最後再到網址列輸入你的ip位置後加上`/phpmyadmin`即可




