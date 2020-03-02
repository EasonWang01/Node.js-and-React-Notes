# Web開發進階Bug

## 1.有關Refer導致拿不到圖片

有時imgur網站當request發出帶有refer等會判定為不安全，所以去get圖片時會無法取回來顯示

例如:`<img src="i.imgur....."> </img>` [http://www.tulinkeji.com/i/24942.html](http://www.tulinkeji.com/i/24942.html)

只要把整個網站的refer拿掉即可 [http://stackoverflow.com/questions/6817595/remove-http-referer](http://stackoverflow.com/questions/6817595/remove-http-referer)

