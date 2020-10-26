# 地理位置

存取權限有分兩種，選擇一種加入即可：

![](../../.gitbook/assets/jie-tu-20201026-shang-wu-10.58.09.png)

{% embed url="https://developers.google.com/maps/documentation/android-sdk/location?hl=zh-tw\#location\_permissions" %}

### 使用：[https://github.com/Agontuk/react-native-geolocation-service](https://github.com/Agontuk/react-native-geolocation-service)

新增完 AndroidMenifest 權限後可如下使用，記得要先獲得[權限](https://reactnative.dev/docs/permissionsandroid)。

```javascript
  async componentDidMount() {
    const grantedLocation = await PermissionsAndroid.request(
      PermissionsAndroid.PERMISSIONS.ACCESS_FINE_LOCATION,
    );
    if (grantedLocation) {
      Geolocation.getCurrentPosition(
        (position) => {
          console.log(position);
        },
        (error) => {
          // See error code charts below.
          console.log(error.code, error.message);
        },
        {enableHighAccuracy: true, timeout: 15000, maximumAge: 10000},
      );
    }
  }
```

