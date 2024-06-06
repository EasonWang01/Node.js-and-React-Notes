---
description: 環境變數
---

# 設置 Postman 環境變數

讓不同環境有不同的 host url

> 讓我們可以去右上方直接選擇環境，又可以針對不同 collection 設置

<figure><img src="../../.gitbook/assets/截圖 2024-06-06 上午11.30.13.png" alt=""><figcaption></figcaption></figure>

在左側 Collection -> Edit -> Pre-request script

```javascript
let base_url;
switch (pm.environment.name){
    case "local":
        base_url = "http://localhost:3000/api/v1";
        break;
    case "uat":
        base_url = "http://...:8111/api/v1";
        break;
    case "production":
        base_url = "https://.../api/v1";
        break;    
}

pm.environment.set("base_url", base_url);
```

之後每次請求 URL 欄位的 \{{base\_url\}} 會被對應更改



## 登入後自動設置 JWT Token

在登入的 API -> Test 加上以下

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Content-Type is application/json", function () {
    pm.response.to.have.header("Content-Type", "application/json; charset=utf-8");
});

var jsonData = pm.response.json();
pm.environment.set("jwt", jsonData.token);

console.log(pm.environment.get('jwt'), jsonData.token)
```

<figure><img src="../../.gitbook/assets/截圖 2024-06-06 上午11.33.17.png" alt=""><figcaption></figcaption></figure>

之後在其他需要 JWT Token header 的 API 加上：

<figure><img src="../../.gitbook/assets/截圖 2024-06-06 上午11.34.08.png" alt=""><figcaption></figcaption></figure>

代碼內要使用 Bearer Token

```typescript
  private extractTokenFromHeader(request: Request): string | undefined {
    const [type, token] = request.headers.authorization?.split(' ') ?? [];
    return type === 'Bearer' ? token : undefined;
  }
```
