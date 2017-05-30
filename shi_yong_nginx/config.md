# try\_file

有關try\_file，他會找目錄下的文件，有時寫了 location  ，但路由總是只有\可以執行，可以看看是不是try\_file沒有刪掉，因為他會優先執行然後沒找到就回傳404，另外可能不經意把自己的源碼給露出了

ex:

```
try_files $uri $uri/ =404;
```

[https://servers.ustclug.org/2014/09/nginx-try\_files-fallacy/](https://servers.ustclug.org/2014/09/nginx-try_files-fallacy/)



# Underscore header

有時我們幫header取名時有\_這時會被nginx預設擋下，我們需加上

```
underscores_in_headers on;
```

https://serverfault.com/questions/586970/nginx-is-not-forwarding-a-header-value-when-using-proxy-pass

