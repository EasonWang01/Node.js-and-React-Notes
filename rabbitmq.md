# RabbitMQ

## 安裝

```
sudo apt-get install rabbitmq-server
```

> 安裝完後會自動啟動

#### 查看狀態

```
rabbitmqctl status
```

## 開啟網頁UI

```
rabbitmq-plugins enable rabbitmq_management
```

預設會有一個使用者，帳號和密碼都是Guest，但只限制localhost連線。

我們可以如下新增一個使用者，即可開放外網連線 :

以下使用使用者: test 密碼: test為範例。

> 1. Add a new/fresh user, say user ‘test’ and password ‘test’
>
> ```
> rabbitmqctl add_user test test
> ```
>
> 1. Give administrative access to the new access
>
> ```
> rabbitmqctl set_user_tags test administrator
> ```
>
> 1. Set permission to newly created user
>
> ```
> rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
> ```

![](/assets/asdw23.png)

## 使用Node.js連線

http://www.squaremobius.net/rabbit.js/

