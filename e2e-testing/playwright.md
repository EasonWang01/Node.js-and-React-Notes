---
description: https://playwright.dev/
---

# PlayWright

以下為為 Microsoft PlayWright 測試工具教學。此為微軟推出的測試工具，包含錄製自動產生測試代碼與 trace 頁面查看測試的每個步驟與頁面結果，並且測試執行時間縮短為 cypress 的一半。

PlayWright 為免費使用安裝的工具，很適合用來自動化測試或是和 CI 工具整合。

## 安裝

```
npm i @playwright/test
```

## 範例

我們用 React 頁面為例：

```javascript
import React, { useState } from 'react';
import './App.css';

function App() {
  const [inputValue, setInputValue] = useState('');
  const [submittedText, setSubmittedText] = useState('');

  const handleInputChange = (e) => {
    setInputValue(e.target.value);
  };

  const handleSubmit = () => {
    setSubmittedText(inputValue);
    setInputValue(''); // Clear the input field after submit
  };

  return (
    <div>
      <h1>Sample React Component</h1>
      <input
        type="text"
        placeholder="Enter some text"
        value={inputValue}
        onChange={handleInputChange}
      />
      <button onClick={handleSubmit}>Submit</button>
      {submittedText && <p>Submitted Text: {submittedText}</p>}
    </div>
  );
}

export default App;
```

創建一個 Test folder 裡面包含檔案：SampleComponent.spec.js

```javascript
const { test, expect } = require('@playwright/test');

test.describe('SampleComponent', () => {
  test('should submit text and display it correctly', async ({ page }) => {
    // Navigate to the page where your component is rendered
    await page.goto('http://localhost:3000'); // Change URL to your local app

    // Check if the input field, button, and text are present
    const input = page.locator('input[placeholder="Enter some text"]');
    const button = page.locator('button:has-text("Submit")');
    const header = page.locator('h1');
    
    await expect(header).toHaveText('Sample React Component');

    // Fill the input field
    await input.fill('Hello, Playwright!');

    // Click the submit button
    await button.click();

    // Verify that the submitted text is displayed correctly
    const submittedText = page.locator('p');
    await expect(submittedText).toHaveText('Submitted Text: Hello, Playwright!');

    // Ensure the input field is cleared after submit
    await expect(input).toHaveValue('');
  });
});
```

## 使用 Headed 模式

```
npx playwright test --headed
```

> playWright 預設為 headless 模式，不開啟瀏覽器
>
> Headed 必須先使用 npx playwright install\`安裝 chromium

## 開啟錄製功能產生測試代碼

```
npx playwright codegen http://localhost:3000
```

使用後會出現一除錯視窗上面顯示我們在頁面上操作產生的測試代碼

<figure><img src="../.gitbook/assets/截圖 2024-09-21 下午10.03.47.png" alt=""><figcaption></figcaption></figure>

## 產生 Trace

```
npx playwright test --trace on
```

之後會產生資料夾 test-results，在在更裡面的測試資歷夾會有 trace.zip，使用以下指令打開可以看到詳細測試執行流程

> 路徑名稱記得要更改為自己的 trace 產生的資料夾名稱

```
npx playwright show-trace ./test-results/tests-SampleComponent-Samp-xt-and-display-it-correctly/trace.zip
```

<figure><img src="../.gitbook/assets/截圖 2024-09-21 下午10.06.48.png" alt=""><figcaption></figcaption></figure>

## HTML 測試報告

```
npx playwright test --reporter=html
```

<figure><img src="../.gitbook/assets/截圖 2024-09-21 下午10.16.40.png" alt=""><figcaption></figcaption></figure>

## 使用內建 VS Code 工具

安裝插件 ![](<../.gitbook/assets/截圖 2024-09-21 下午10.20.53.png>)

之後記得把測試資料夾命名為 e2e，這個插件才能讀取到

或是可以使用它的初始化命令：

```
npm init playwright@latest --yes -- --quiet --browser=chromium --browser=firefox --browser=webkit --lang=js
```

之後就可以直接從介面控制測試

\
![](<../.gitbook/assets/截圖 2024-09-21 下午10.22.56.png>)
