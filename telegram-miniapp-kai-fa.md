# Telegram MiniAPP 開發

## 設置 MiniAPP

1. 在 Telegram 內搜尋 botfather

![](<.gitbook/assets/截圖 2024-11-15 上午9.46.29.png>)

2. 左下角選單選擇 newbot 創建機器人

<div align="left">

<figure><img src=".gitbook/assets/截圖 2024-11-15 上午9.47.34.png" alt="" width="188"><figcaption></figcaption></figure>

</div>

3. 創建好機器人後，一樣到選單，選擇創建 newapp

![](<.gitbook/assets/截圖 2024-11-15 上午9.48.59.png>)

4. 之後他會要你選擇一個綁定的 bot，選剛才創件的 bot，然後上傳 640x360 pixels 圖片，然後放上 miniApp 部署的 https 網頁 URL (可以先用 ngrok 方便開發)，之後他就會給你 miniAPP url 點擊後會開起 miniApp。

## 開發 MiniApp

我們可以使用模板來創建專案：

{% embed url="https://github.com/telegram-mini-apps-dev/vite-boilerplate?tab=readme-ov-file" %}

專案跑起來後，去下載 ngrok [https://ngrok.com/](https://ngrok.com/) ，接著就是前面步驟的設置 miniApp URL 為 ngrok 提供的 HTTPS URL (記得要 HTTPS)。

接著就可以參考以下文件進行開發 &#x20;

[https://core.telegram.org/bots/webapps#initializing-mini-apps](https://core.telegram.org/bots/webapps#initializing-mini-apps)

## 驗證用戶

用戶登入 miniApp 時我們可以獲得 initData 裡面包含用戶名稱與 id 等等，當中包含 miniApp 幫用戶自動產生的 hash，而我們要用加密驗證的方式來將 tg bot 與這個 hash 驗證，確定實際上是這個用戶在操作 APP，可使用以下代碼來驗正

前端：

```javascript
  useEffect(() => {
    const webApp = window?.Telegram.WebApp;
    console.log('WebAppInitData:', webApp.initData); // querystring 型態
  }, [window?.Telegram.WebApp]);
```

後端：

```typescript
  async signinUser(webInitData: string, ip: string) {
    function validateTelegramWebAppData(initData, botToken) {
      // Step 1: Parse initData into an object
      const params = new URLSearchParams(initData);
      const dataObj = {};
      params.forEach((value, key) => {
        dataObj[key] = value;
      });
    
      // Step 2: Extract and remove the hash
      const receivedHash = dataObj['hash'];
      delete dataObj['hash'];
    
      // Step 3: Sort the keys and construct dataCheckString
      const dataCheckString = Object.keys(dataObj)
        .sort()
        .map(key => `${key}=${dataObj[key]}`)
        .join('\n');
    
      // Step 4: Create a secret key using the bot token
      const secretKey = createHmac('sha256', 'WebAppData')
        .update(botToken)
        .digest();
    
      // Step 5: Hash the dataCheckString using the secret key
      const calculatedHash = createHmac('sha256', secretKey)
        .update(dataCheckString)
        .digest('hex');
    
      // Step 6: Compare calculated hash with received hash (both should be lowercase)
      return { isOk: calculatedHash === receivedHash, dataObj };
    }
    if (!webInitData) {
      throw new BadRequestException('webInitData is required');
    }
    
    const botToken = process.env['TG_BOT_TOKEN']; // 這邊記得在 .env 檔案放上 TG_BOT_TOKEN 
    
    const { isOk, dataObj } = validateTelegramWebAppData(webInitData, botToken)
    if (!isOk) {
      throw new BadRequestException('Invalid tg sigature');
    }
    const userData = JSON.parse(dataObj['user']);
    
    if (!botToken) {
      console.log('Something wrong for tg bot token');
      throw new BadRequestException('Something wrong for tg bot token');
    }

    const userId = String(userData['id']);
    const username = String(userData['username']);
    // 到了這邊已經驗證用戶成功，可以使用 userData 內的資料來進行操作
  }
```

{% embed url="https://core.telegram.org/bots/webapps#validating-data-received-via-the-mini-app" %}

## 其他工具

手機端開發時的 console.log 工具 [https://github.com/liriliri/eruda](https://github.com/liriliri/eruda)

> 會在右下角有按鈕，打開可以看到 log
