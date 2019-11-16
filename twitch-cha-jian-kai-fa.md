# Twitch 插件開發

[https://dev.twitch.tv/docs/extensions/](https://dev.twitch.tv/docs/extensions/)

[https://dev.twitch.tv/console/](https://dev.twitch.tv/console/)

1. 先創建頻道
2. 到dev.twitch 註冊開發者
3. 到Monetinzation 開啟收入功能

![](/assets/螢幕快照 2019-11-16 下午12.29.54.png)

> 其中稅籍登記部分地址記得要填寫相同，不然送出會錯誤

## 開發

#### 1.下載OBS

#### 2.下載Twitch Developer Rig

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

