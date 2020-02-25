# React Native 相機與圖片庫存取

https://github.com/react-community/react-native-image-picker

```js
yarn add react-native-image-picker
react-native link react-native-image-picker

然後記得重啟

react-native run-android
```

> 記得在Androidmanifest 加上 camera permission
>
> ```
> <uses-permission android:name="android.permission.CAMERA" />
> ```

然後

```js
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



