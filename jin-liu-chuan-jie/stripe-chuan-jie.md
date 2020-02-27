# Stripe

stripe為一個金流串接服務提供者，其特點在於串接簡單。

#### 1.先到其網站註冊帳號：[https://stripe.com](https://stripe.com)

> 需要具有其許可國家內的銀行帳號與手機。

#### 2.接著就是試著串接API

* 程式範例:[https://stripe.com/docs/checkout/express](https://stripe.com/docs/checkout/express)
* 商店Dashboard:[https://dashboard.stripe.com](https://dashboard.stripe.com)

> 建立商店後會可以進入一個Dashboard介面，在左下方把Test打開即可進入測試模式。
>
> ![](/assets/Screen Shot 2018-08-17 at 3.13.45 PM.png)

#### 3.程式範例：

Server.js

```js
const keyPublishable = process.env.PUBLISHABLE_KEY;
const keySecret = process.env.SECRET_KEY;

const app = require("express")();
const stripe = require("stripe")(keySecret);

app.set("view engine", "pug");
app.use(require("body-parser").urlencoded({extended: false}));

app.get("/", (req, res) =>
  res.render("index.pug", {keyPublishable}));

app.post("/charge", (req, res) => {
  let amount = 500;

  stripe.customers.create({
     email: req.body.stripeEmail,
    source: req.body.stripeToken
  })
  .then(customer => {
    stripe.charges.create({
      amount,
      description: "Sample Charge",
         currency: "usd",
         customer: customer.id
    })
    console.log(req.body.stripeToken)
    console.log(req.body.stripeToken)
  }
  )
  .then(charge => res.render("charge.pug"));
});

app.listen(4567);
```

views/index.pug

```pug
<html>
<head>
<script src="https://checkout.stripe.com/v2/checkout.js"></script></head>

<body>
    <form action="/charge" method="post">
        <article><label>Amount: $5.00</label></article>
        <script class="stripe-button" data-key="pk_test_nk4UQNZM8NWjfvmhKjfZnWav" src="//checkout.stripe.com/v2/checkout.js" data-locale="auto" data-description="Sample Charge" data-amount="500"></script>
    </form>
</body>

</html>
```

views/charge.pug

```html
h2 You successfully paid <strong>$5.00</strong>!
```

## 原理：

要求客戶付款的都在checkout文件：[https://stripe.com/docs/checkout](https://stripe.com/docs/checkout)

#### 1.引入stripe的官方script後，加入一個元件

```html
    <form action="/charge" method="post">
        <article><label>Amount: $5.00</label></article>
        <script class="stripe-button" data-key="pk_test_nk4UQNZM8NWjfvmhKjfZnWav" src="//checkout.stripe.com/v2/checkout.js" data-locale="auto" data-description="Sample Charge" data-amount="500"></script>
    </form>
```

> 此元件會產生官方的付款按鈕

#### 2.之後點擊按鈕後會要求填入信用卡資訊與email

![](/assets/Screen Shot 2018-08-17 at 3.22.28 PM.png)

#### 3.送出後會發送Request給stripe

然後stripe成功後回傳request給剛才元件上寫的Endpoint，也就是form上寫的action位置

#### 4.接著我們後端App.js接到後會執行

```js
  stripe.customers.create({
     email: req.body.stripeEmail,
    source: req.body.stripeToken
  })
```

產生如下請求

#### ![](/assets/Screen Shot 2018-08-17 at 3.20.39 PM.png)5.最後前一個請求返回後會進行確認請求

```js
    stripe.charges.create({
      amount,
      description: "Sample Charge",
         currency: "usd",
         customer: customer.id
    })
```

![](/assets/Screen Shot 2018-08-17 at 3.26.43 PM.png)

> 以上的請求詳細內容都可以在左側log tab看到
>
> ![](/assets/Screen Shot 2018-08-17 at 3.27.20 PM.png)

# 使用React

可使用此模組：

[https://github.com/stripe/react-stripe-js](https://github.com/stripe/react-stripe-js)

#### 流程：

1.使用官方的元件輸入信用卡號碼與資訊，然後前端呼叫`stripe.createPaymentMethod` 傳給後端

2.後端呼叫並產生`stripe.paymentIntents` 然後將`paymentIntent.client_secret` 傳給前端

3. 前端使用`stripe.confirmCardPayment` 加上剛才的`paymentIntent.client_secret`確認交易

4.成功後再發一個request給後端更新使用者購買狀態

```
yarn add @stripe/react-stripe-js
```

```js
import React from "react";
import "./App.css";
import axios from 'axios';
import StripeCheckout from "react-stripe-checkout";
function App() {
  const onToken = token => {
    axios
      .post(`http://localhost:8081/stripepay`, token)
      .then(response => {
        window.alert("成功", "", "success");
      })
      .catch(err => {
        console.log(err)
        window.alert("失敗，請重試");
      });
  };
  return (
    <div className="App">
        <StripeCheckout stripeKey={"填入public key"} token={(token) => onToken(token)} />
    </div>
  );
}

export default App;
```

Server.js

```js
const express = require('express');
const app = express();
const bodyParser = require('body-parser');
const cors = require('cors')

// create application/json parser
app.use(bodyParser.json());
// create application/x-www-form-urlencoded parser
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cors())

app.post("/stripepay", async (req, res) => {
  console.log(req.body)
  const stripe = require("stripe")("sk_test_LtpwSuF1PsGKLLv3rlVCTZlN");
  let amount = 550;
  const paymentIntent = await stripe.paymentIntents.create({
    amount,
    currency: 'sgd',
    payment_method_types: ['card'],
    metadata: {
      order_id: 6735,
    },
  });
  console.log(paymentIntent)
  res.end(JSON.stringify(paymentIntent))
});

app.listen(8081, () => console.log('app start'));
```

# 確認付款

使用`stripe.paymentIntents.create` 回傳的client secret 並丟給前端，然後前端使用`stripe.confirmCardPayment(clientSecret)`

來判斷

[https://stripe.com/docs/payments/payment-intents/verifying-status](https://stripe.com/docs/payments/payment-intents/verifying-status)

# 客製化按鈕

[https://github.com/stripe/elements-examples](https://github.com/stripe/elements-examples)

[https://stripe.com/docs/stripe-js](https://stripe.com/docs/stripe-js)

