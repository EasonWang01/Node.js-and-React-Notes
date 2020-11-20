# 相機與圖片庫存取

## React Native 相機與圖片庫存取

[https://github.com/react-community/react-native-image-picker](https://github.com/react-community/react-native-image-picker)

```javascript
yarn add react-native-image-picker
react-native link react-native-image-picker

然後記得重啟

react-native run-android
```

> 記得在Androidmanifest 加上 camera permission
>
> ```text
> <uses-permission android:name="android.permission.CAMERA" />
> ```

然後

```javascript
selectImage(){
    const options = {
      title: 'Select Avatar',
      storageOptions: {
        skipBackup: true,
        path: 'images',
      },
    };

    /**
     * The first arg is the options object for customization (it can also be null or omitted for default options),
     * The second arg is the callback which sends object: response (more info in the API Reference)
     */
    ImagePicker.showImagePicker(options, (response) => {
      console.log('Response = ', response);

      if (response.didCancel) {
        console.log('User cancelled image picker');
      } else if (response.error) {
        console.log('ImagePicker Error: ', response.error);
      } else if (response.customButton) {
        console.log('User tapped custom button: ', response.customButton);
      } else {
        const source = { uri: response.uri };

        // You can also display the image using data:
        // const source = { uri: 'data:image/jpeg;base64,' + response.data };

        this.setState({
          avatarSource: source,
        });
      }
    });
  }
```

## 可以裁的 image picker

[https://github.com/ivpusic/react-native-image-crop-picker](https://github.com/ivpusic/react-native-image-crop-picker)

> 之後記得更新 gradle version

簡單上傳 imgur 範例

> 記得先去註冊 [https://api.imgur.com/oauth2/addclient](https://api.imgur.com/oauth2/addclient) 然後改 client ID

```javascript
const handlePicker = async () => {
    try {
      const selectedImage = await ImagePicker.openPicker({
        width: 300,
        height: 400,
        cropping: true,
      });
      const imageBase64 = await ImgToBase64.getBase64String(selectedImage.path);
      console.log(imageBase64);
      const xhttp = new XMLHttpRequest();
      xhttp.open('POST', 'https://api.imgur.com/3/image', true);
      xhttp.setRequestHeader('Content-type', 'application/json');
      xhttp.setRequestHeader('Authorization', 'Client-ID b50a7351eee91f');
      xhttp.send(JSON.stringify({image: imageBase64}));
      xhttp.onreadystatechange = function () {
        if (xhttp.readyState === 4 && xhttp.status === 200) {
          console.log(JSON.parse(xhttp.responseText));
        }
      };
    } catch (err) {
      console.log(err);
    }
  };
```

