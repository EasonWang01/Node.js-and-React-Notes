以下為Android版本

使用[https://github.com/magus/react-native-facebook-login](https://github.com/magus/react-native-facebook-login) 套件

1.主要照著他的步驟  [https://github.com/magus/react-native-facebook-login/blob/master/android/README.md](https://github.com/magus/react-native-facebook-login/blob/master/android/README.md)

> 注意事項:
>
> 1.都設定完了以後記得要重啟dev server和重新run，不然會有錯誤
>
> 2.記得build.gradle的dependence要加在裡面的那個build.gradle而不是外面那個

完成後的AndroidManifest

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.awesomeproject05"
    xmlns:tools="http://schemas.android.com/tools"
    android:versionCode="1"
    android:versionName="1.0">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>

    <uses-sdk
        android:minSdkVersion="16"
        android:targetSdkVersion="22" />

    <application
      android:name=".MainApplication"
      android:allowBackup="true"
      android:label="@string/app_name"
      android:icon="@mipmap/ic_launcher"
      android:theme="@style/AppTheme">
      <activity
        android:name=".MainActivity"
        android:label="@string/app_name"
        android:configChanges="keyboard|keyboardHidden|orientation|screenSize"
        android:windowSoftInputMode="adjustResize">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
      </activity>

        <!--add FacebookActivity-->
        <activity
            tools:replace="android:theme"
            android:name="com.facebook.FacebookActivity"
            android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name"
            android:theme="@android:style/Theme.Translucent.NoTitleBar"/>

        <!--add CustomTabActivity-->
        <activity
            android:name="com.facebook.CustomTabActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="@string/fb_login_protocol_scheme" />
            </intent-filter>
        </activity>


      <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />

        <meta-data
            android:name="com.facebook.sdk.ApplicationId"
            android:value="@string/fb_app_id"/>

    </application>

</manifest>
```

完成後的build.gradle

```
apply plugin: "com.android.application"

import com.android.build.OutputFile

apply from: "../../node_modules/react-native/react.gradle"

def enableSeparateBuildPerCPUArchitecture = false

def enableProguardInReleaseBuilds = false

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.1"

    defaultConfig {
        applicationId "com.awesomeproject05"
        minSdkVersion 16
        targetSdkVersion 22
        versionCode 1
        versionName "1.0"
        ndk {
            abiFilters "armeabi-v7a", "x86"
        }
    }
    splits {
        abi {
            reset()
            enable enableSeparateBuildPerCPUArchitecture
            universalApk false  // If true, also generate a universal APK
            include "armeabi-v7a", "x86"
        }
    }
    buildTypes {
        release {
            minifyEnabled enableProguardInReleaseBuilds
            proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"
        }
    }
    // applicationVariants are e.g. debug, release
    applicationVariants.all { variant ->
        variant.outputs.each { output ->
            // For each separate APK per architecture, set a unique version code as described here:
            // http://tools.android.com/tech-docs/new-build-system/user-guide/apk-splits
            def versionCodes = ["armeabi-v7a":1, "x86":2]
            def abi = output.getFilter(OutputFile.ABI)
            if (abi != null) {  // null for the universal-debug, universal-release variants
                output.versionCodeOverride =
                        versionCodes.get(abi) * 1048576 + defaultConfig.versionCode
            }
        }
    }
}

dependencies {
    compile fileTree(dir: "libs", include: ["*.jar"])
    compile "com.android.support:appcompat-v7:23.0.1"
    compile "com.facebook.react:react-native:+"  // From node_modules
    compile project(':react-native-facebook-login')
}

// Run this once to be able to run the application with BUCK
// puts all compile dependencies into folder libs for BUCK to use
task copyDownloadableDepsToLibs(type: Copy) {
    from configurations.compile
    into 'libs'
}
```

完成後的string.sml

```
<resources>
    <string name="app_name">AwesomeProject05</string>
    <string name="fb_app_id">730254410464547</string>
    <string name="fb_login_protocol_scheme">fb730254410464547</string>
</resources>
```

完成後的index.js

```
import React, { Component } from 'react';
import {FBLogin, FBLoginManager} from 'react-native-facebook-login';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

export default class AwesomeProject05 extends Component {
  componentDidMount() {
    console.log(123)
  }
  render() {
    return (
      <View style={styles.container}>
        <FBLogin style={{ marginBottom: 10, }}
        ref={(fbLogin) => { this.fbLogin = fbLogin }}
        permissions={["email","user_friends"]}
        loginBehavior={FBLoginManager.LoginBehaviors.Native}
        onLogin={function(data){
          console.log("Logged in!");
          console.log(data);
          //this.setState({ user : data.credentials });
        }}
        onLogout={function(){
          console.log("Logged out.");
          //this.setState({ user : null });
        }}
        onLoginFound={function(data){
          console.log("Existing login found.");
          console.log(data);
          //this.setState({ user : data.credentials });
        }}
        onLoginNotFound={function(){
          console.log("No user logged in.");
          //this.setState({ user : null });
        }}
        onError={function(data){
          console.log("ERROR");
          console.log(data);
        }}
        onCancel={function(){
          console.log("User cancelled.");
        }}
        onPermissionsMissing={function(data){
          console.log("Check permissions!");
          console.log(data);
        }}
      />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});

AppRegistry.registerComponent('AwesomeProject05', () => AwesomeProject05);
```

也可自己寫一個按鈕

```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import { FBLogin, FBLoginManager } from 'react-native-facebook-login';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  Button
} from 'react-native';

export default class AwesomeProject05 extends Component {
  componentDidMount() {
    console.log(123)
  }

  FBlogin() {
    FBLoginManager.loginWithPermissions(["email", "user_friends"], function (error, data) {
      if (!error) {
        console.log("Login data: ", data);
      } else {
        console.log("Error: ", error);
      }
    })
  }
  render() {
    return (
      <View style={styles.container}>

        <Button
          onPress={() => this.FBlogin()}
          title="Learn More"
          color="#841584"
          accessibilityLabel="Learn more about this purple button"
        />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    height: 20,
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});

AppRegistry.registerComponent('AwesomeProject05', () => AwesomeProject05);

```



