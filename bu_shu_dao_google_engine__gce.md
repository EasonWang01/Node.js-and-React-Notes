# 部屬到Google Engine (GCE)

到資訊主頁後點選Compute Engine

現在有兩個月試用，以及300美元的
免費額度

他的SSH可以直接用瀏覽器連線

##查看現在占用的PORT

`sudo netstat -tulpn`

#安裝Mysql
https://community.linuxmint.com/tutorial/view/486

1`sudo apt-get install apache2`

之後在外面電腦輸入你的VPS IP即可看到default畫面

接著跟著安裝一些套件

再來要改config檔案時可使用vim

輸入 `sudo vim 加檔案路徑即可`

之後記得restart `sudo /etc/init.d/apache2 restart`

最後再到網址列輸入你的ip位置後加上`/phpmyadmin`即可



##更改網域名稱DNS


參考此文章跟影片

https://www.youtube.com/watch?v=8l--TJabo2M

http://howtodosteps.blogspot.tw/2015/08/how-to-assigntransfer-godaddy-domain.html

之後已godaddy為範例，在我的網域=>管理DNS  下面的域名伺服器新增如下(先到GCE查看)如圖

![1](https://cloud.githubusercontent.com/assets/11001914/17292905/68992eca-581f-11e6-9612-806d5f78b0be.png)

![2](https://cloud.githubusercontent.com/assets/11001914/17292906/6a0ca958-581f-11e6-8fce-af92d16997f3.png)



之後等待48小時。