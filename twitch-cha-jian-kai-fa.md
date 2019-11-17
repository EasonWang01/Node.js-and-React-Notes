# Twitch 插件開發

[https://dev.twitch.tv/docs/extensions/](https://dev.twitch.tv/docs/extensions/)

[https://dev.twitch.tv/console/](https://dev.twitch.tv/console/)

1. 先創建頻道
2. 到dev.twitch 註冊開發者
3. 到Monetinzation 開啟收入功能

> 開發因為會發request 到 http 所以必須使用http://twitch.tv測試，然後開啟url右側的安全性同意，或是使用Firefox developer Edition

![](/assets/螢幕快照 2019-11-16 下午12.29.54.png)

> 其中稅籍登記部分地址記得要填寫相同，不然送出會錯誤

## 開發

#### 1.下載OBS

#### 2.下載Twitch Developer Rig

> ```
> twitch.rig.log(123); // 要用此，並且開啟直播才能console到
> ```

#### 3.下載設 Twitch Hello world範例，並設定如下

#### ![](/assets/螢幕快照 2019-11-16 下午12.32.25.png)4.啟動前端與後端 server後可看到畫面![](/assets/螢幕快照 2019-11-16 下午12.33.14.png)

#### 5. 到 twitch &gt; setting &gt; channel &gt; channel and video &gt; Extension

![](/assets/螢幕快照 2019-11-16 下午12.35.55.png)可在此將範例加入

#### 6. 設定endpoint 為 http

![](/assets/螢幕快照 2019-11-16 下午12.38.45.png)

#### 6.使用[http://twitch.tv](http://twitch.tv) 登入，記得點選url右側的安全性選項

![](/assets/螢幕快照 2019-11-16 下午12.39.39.png)

## 開發

有分三種類型的插件

1. panel
2. component \(partscreen\)
3. overlay \(fullscreen\)

#### ![](/assets/螢幕快照 2019-11-16 下午12.41.15.png)

#### 範例程式：

[https://github.com/twitchdev/extensions-hello-world](https://github.com/twitchdev/extensions-hello-world)

> 檔案上面分別為不同類型的插件，不要改錯檔案，改完按重新整理即可
>
> services/backend.js 為後端程式，後端需要JWT Auth 只有在 頻道上才可測試 `Twitch.onAuth` function

![](/assets/螢幕快照 2019-11-16 下午12.41.46.png)

#### 引入函式庫

要使用Twitch API 只要使用以下即可

```js
<script src="https://extension-files.twitch.tv/helper/v1/twitch-ext.min.js"></script>
```

#### Bit 開發

Bit 為儲值點數的單位，可用以下方法

```js
const twitch = window.Twitch.ext;
document.getElementById('skuBtn').addEventListener('click', () => {
  twitch.bits.useBits(this.state.sku)
})
```

取得develop rig內設定可購買的bit產品

```js
twitch.bits.getProducts().then(product => {
  twitch.rig.log(product);
  console.log(product)
})
```

完成交易

```js
twitch.bits.onTransactionComplete(function (transaction) {twitch.rig.log("onTransactionComplete() fired, receivedtransactionReceipt: " + transaction.transactionReceipt);});
```

# 使用臉部探測的Extension 範例

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8" />
  <title>Demo</title>
  <!-- Load TensorFlow.js-->
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@1.0.0/dist/tf.min.js"></script>
  <!-- Load the coco-ssd model. -->
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/coco-ssd"></script>
  <!-- Load React. -->
  <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>

  <script src="https://unpkg.com/babel-standalone@6.26.0/babel.min.js"></script>
  <script src="https://extension-files.twitch.tv/helper/v1/twitch-ext.min.js"></script>
  <style>
  </style>
</head>

<body>
  <!-- Load our React component. -->
  <script src="detect.js" type="text/babel"></script>

  <!-- We will put our React component inside this div. -->
  <div id="root"></div>
</body>
```

detect.js

```js
class App extends React.Component {
  // reference to both the video and canvas
  videoRef = React.createRef();
  canvasRef = React.createRef();
  constructor() {
    super();
    this.state = {
      shameShieldsNum: 100,
      sku: '',
      activeTimer: false,
    }
  }

  // we are gonna use inline style
  styles = {
    position: 'fixed',
    top: 150,
    left: 150,
    display: 'none',
  };


  detectFromVideoFrame = (model, video) => {
    model.detect(video).then(predictions => {
      this.showDetections(predictions);

      requestAnimationFrame(() => {
        this.detectFromVideoFrame(model, video);
      });
    }, (error) => {
      console.log("Couldn't start the webcam")
      console.error(error)
    });
  };

  showDetections = predictions => {
    const context = this;
    const ctx = this.canvasRef.current.getContext("2d");
    ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
    const font = "24px helvetica";
    ctx.font = font;
    ctx.textBaseline = "top";
    var timer = 3;
    if (predictions.length > 1) {
      // more then two person
      if (document.getElementById('mathImg').style.display === 'none') {
        console.log('Alert!! wife comes in');
        document.getElementById('mathImg').style.display = 'block';
        context.setState({ shameShieldsNum: this.state.shameShieldsNum - 1 });
        if (!context.state.activeTimer) {
          context.setState({ activeTimer: true });
          var shieldTimer = setInterval(() => {
            console.log(timer)
            if (timer <= 0) {
              clearInterval(shieldTimer);
              context.setState({ activeTimer: false });
            }
            timer -= 1;
          }, 1000);
        }
      }
    } else {
      if (!context.state.activeTimer) {
        document.getElementById('mathImg').style.display = 'none';
      }
      console.log('Safe');
    }
    predictions.forEach(prediction => {
      const x = prediction.bbox[0];
      const y = prediction.bbox[1];
      const width = prediction.bbox[2];
      const height = prediction.bbox[3];
      // Draw the bounding box.
      ctx.strokeStyle = "#2fff00";
      ctx.lineWidth = 1;
      ctx.strokeRect(x, y, width, height);
      // Draw the label background.
      ctx.fillStyle = "#2fff00";
      const textWidth = ctx.measureText(prediction.class).width;
      const textHeight = parseInt(font, 10);
      // draw top left rectangle
      ctx.fillRect(x, y, textWidth + 10, textHeight + 10);
      // draw bottom left rectangle
      ctx.fillRect(x, y + height - textHeight, textWidth + 15, textHeight + 10);

      // Draw the text last to ensure it's on top.
      ctx.fillStyle = "#000000";
      ctx.fillText(prediction.class, x, y);
      ctx.fillText(prediction.score.toFixed(2), x, y + height - textHeight);
    });
  };

  componentDidMount() {
    document.getElementById('showCanvasT').addEventListener('click', () => {
      if (document.getElementById('canvasT').style.display === 'block') {
        document.getElementById('canvasT').style.display = 'none'
      } else {
        document.getElementById('canvasT').style.display = 'block'
      }
    })

    const context = this;
    document.getElementById('mathImg').style.display = 'none';
    if (navigator.mediaDevices.getUserMedia || navigator.mediaDevices.webkitGetUserMedia) {
      // define a Promise that'll be used to load the webcam and read its frames
      const webcamPromise = navigator.mediaDevices
        .getUserMedia({
          video: true,
          audio: false,
        })
        .then(stream => {
          // pass the current frame to the window.stream
          window.stream = stream;
          // pass the stream to the videoRef
          this.videoRef.current.srcObject = stream;

          return new Promise(resolve => {
            this.videoRef.current.onloadedmetadata = () => {
              resolve();
            };
          });
        }, (error) => {
          console.log("Couldn't start the webcam")
          console.error(error)
        });

      // define a Promise that'll be used to load the model
      const loadlModelPromise = cocoSsd.load();

      // resolve all the Promises
      Promise.all([loadlModelPromise, webcamPromise])
        .then(values => {
          this.detectFromVideoFrame(values[0], this.videoRef.current);
        })
        .catch(error => {
          console.error(error);
        });
    }

    // Twitch API
    console.log('onload')
    
    const twitch = window.Twitch.ext;
    document.getElementById('skuBtn').addEventListener('click', () => {
      twitch.bits.useBits(this.state.sku)
    })


    twitch.bits.getProducts().then(product => {
      console.log(product)
      context.setState({ sku: product[0].sku })
    })

    twitch.bits.onTransactionComplete(function (transaction) {
      console.log(transaction);
      context.setState({ shameShieldsNum: context.state.shameShieldsNum + 100 });
    })
  }

  // here we are returning the video frame and canvas to draw,
  // so we are in someway drawing our video "on the go"
  render() {
    return (
      <div>
        <video
          style={this.styles}
          autoPlay
          muted
          ref={this.videoRef}
          width="720"
          height="600"
        />
        <img style={{ backgroundSize: '100%', width: '1000px', height: '700px' }} id="mathImg" src="images/code.png"></img>
        <canvas id="canvasT" style={this.styles} ref={this.canvasRef} width="720" height="650" />
        <h1 style={{ color: "white" }}>剩餘護盾 (shame shields): {this.state.shameShieldsNum}</h1>
        <button id="skuBtn">儲值護盾 (Buy shield)</button>
        <button id="showCanvasT">檢測 (Inspect)</button>
      </div>
    );
  }
}

const domContainer = document.querySelector('#root');
ReactDOM.render(React.createElement(App), domContainer);
```

> 以上範例要使用Firefox developer Edition 開啟



