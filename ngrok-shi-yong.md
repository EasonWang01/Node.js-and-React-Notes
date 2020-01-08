# Ngrok 使用

[https://dashboard.ngrok.com/get-started](https://dashboard.ngrok.com/get-started)

可以幫你把主機的某個port，變公開的，並給你一串公開網址，可以發請求，類似port forwarding，

可用於測試webhook，因為可快速讓 local port 變 https 網址

## 使用

```
wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
unzip ./ngrok.zip
./ngork http <port>
```

## 在背景跑

```
./ngrok http <port> > /dev/null &
export WEBHOOK_URL="$(curl http://localhost:4040/api/tunnels | jq ".tunnels[0].public_url")"
echo $WEBHOOK_URL
```

> 要關閉可使用ps -A \| grep ngrok 然後再 kill -9 &lt;pid&gt;

## 查看dashboard

```
http://localhost:4040
```



