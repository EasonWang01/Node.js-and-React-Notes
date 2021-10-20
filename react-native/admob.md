# Admob

建議可用 firebase 出的模組。

{% embed url="https://rnfirebase.io/admob/usage" %}

1. 申請好 admob 帳號後新增應用程式，然後新增廣告單元，之後會取得 App ID 與 Ad ID

![](<../.gitbook/assets/截圖 2021-03-04 下午5.19.09.png>)

2.在專案根目錄新增 firebase.json

```
{
  "react-native": {
    "admob_android_app_id": "ca-app-pub-xxxxxxxx~xxxxxxxx",
    "admob_ios_app_id": "ca-app-pub-xxxxxxxx~xxxxxxxx"
  }
}
```

3\. 初始化

```javascript
import admob, { MaxAdContentRating } from '@react-native-firebase/admob';

admob()
  .setRequestConfiguration({
    // Update all future requests suitable for parental guidance
    maxAdContentRating: MaxAdContentRating.PG,

    // Indicates that you want your content treated as child-directed for purposes of COPPA.
    tagForChildDirectedTreatment: true,

    // Indicates that you want the ad request to be handled in a
    // manner suitable for users under the age of consent.
    tagForUnderAgeOfConsent: true,
  })
  .then(() => {
    // Request config successfully set!
  });
```

4.顯示廣告以全版的 reward ad 為例

```javascript
import { RewardedAd, TestIds } from '@react-native-firebase/admob';

const adUnitId = __DEV__ ? TestIds.INTERSTITIAL : 'ca-app-pub-xxxxxxxxxxxxx/yyyyyyyyyyyyyy';

const rewarded = RewardedAd.createForAdRequest(adUnitId, {
  requestNonPersonalizedAdsOnly: true,
  keywords: ['fashion', 'clothing'],
});

  componentDidMount() {
    const eventListener = rewarded.onAdEvent((type, error, reward) => {
      if (type === RewardedAdEventType.LOADED) {
        console.log('ad loaded');
        rewarded.show();
      }

      if (type === RewardedAdEventType.EARNED_REWARD) {
        console.log('User earned reward of ', reward);
      }
    });

handleShowAd() {
 rewarded.load();
}
......
<Button onPress={() => this.handleShowAd()}>
    <Text>觀看</Text>
</Button>
```
