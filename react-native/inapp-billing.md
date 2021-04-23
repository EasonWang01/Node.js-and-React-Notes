# InApp Billing

使用此模組：

[https://github.com/dooboolab/react-native-iap/](https://github.com/dooboolab/react-native-iap/blob/master/IapExample/package.json)

官方範例：

{% embed url="https://github.com/dooboolab/react-native-iap/blob/master/IapExample/" %}

## 安裝

```text
yarn add react-native-iap
```

### Android 設置

1.新增 license key

![](../.gitbook/assets/jie-tu-20210423-xia-wu-2.17.54.png)

![](../.gitbook/assets/jie-tu-20210423-xia-wu-2.16.56.png)

2.上傳 app 到封閉測試群組

![](../.gitbook/assets/jie-tu-20210423-xia-wu-2.18.29.png)

3.新增產品

![](../.gitbook/assets/jie-tu-20210423-xia-wu-2.19.39.png)

4.新建  License Testing 使用者，並且測試的手機 Google play 要登入並選擇使用該帳號

![](../.gitbook/assets/jie-tu-20210423-xia-wu-2.12.59.png)

5.確保 build version 與 封閉測試的 APP 版本相同

![](../.gitbook/assets/jie-tu-20210423-xia-wu-2.22.14.png)

6.使用 release key 測試， react-native 可用如下指令

```text
npx react-native run-android --variant=release
```

#### 其他問題可參考：

{% embed url="https://stackoverflow.com/questions/11068686/this-version-of-the-application-is-not-configured-for-billing-through-google-pla" %}

{% embed url="https://developer.android.com/google/play/billing/billing\_reference.html" %}

{% embed url="https://github.com/dooboolab/react-native-iap/issues/1308" %}



