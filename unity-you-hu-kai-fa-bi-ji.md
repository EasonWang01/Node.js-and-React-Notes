# Unity 遊戲開發筆記

### 基礎物理

[https://www.youtube.com/watch?v=dLYTwDQmjdo](https://www.youtube.com/watch?v=dLYTwDQmjdo)

### 獲取物件

[https://home.gamer.com.tw/creationDetail.php?sn=3607470](https://home.gamer.com.tw/creationDetail.php?sn=3607470)

### 產生隨機數：

[https://docs.unity3d.com/ScriptReference/Random.Range.html](https://docs.unity3d.com/ScriptReference/Random.Range.html)

### 監聽鍵盤按下：

[https://docs.unity3d.com/ScriptReference/Input.GetButtonDown.html](https://docs.unity3d.com/ScriptReference/Input.GetButtonDown.html)

### 監聽鍵盤持續按下：

https://answers.unity.com/questions/412117/do-something-while-key-is-pressed-and-held-down.html

### 把程式的變量加入到介面為可調整：

[https://blog.csdn.net/yangyong0717/article/details/71512251](https://blog.csdn.net/yangyong0717/article/details/71512251)

### Debug Android 時顯示 log:

`adb logcat -s Unity PackageManager dalvikvm DEBUG`

### 讓裝置不睡眠：

[Screen.sleepTimeout](https://docs.unity3d.com/ScriptReference/Screen-sleepTimeout.html) = [SleepTimeout.NeverSleep](https://docs.unity3d.com/ScriptReference/SleepTimeout.NeverSleep.html);

### 手機陀螺儀使用：

[https://www.youtube.com/watch?v=fsEkZLBeTJ8](https://www.youtube.com/watch?v=fsEkZLBeTJ8)

### 在 android 上執行遊戲：

(使用裝置 USB 連上電腦)

1. Edit -> project setting -> product name 改變 app 名稱，也可改變執行後的 orientation

2\. File-> build setting 選擇 android (如果不能選要回到 Unity Hub 改為 android build 然後重開 unity)

3\. File -> build and Run

> 如果不小心在手機上直接刪除了測試中的 app 可以用 adb 移除套件或是改變 product name 也可以

4\. 如果只有改程式碼可以勾選 development build 之後下面會出現 script only build 然後點選 patch and run

![](<.gitbook/assets/截圖 2021-06-19 上午10.12.04.png>)

### UI 字體模糊:

### [http://answers.unity.com/answers/1399750/view.html](http://answers.unity.com/answers/1399750/view.html) 

### UI 元件 canvas 沒顯示：&#x20;

重新到 build setting 選擇 Build and Run 即可。另外可查看是否大小需要調整。

### 重置場景：

```java
using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement;

public class ResetBtn : MonoBehaviour {
	public Button yourButton;

	void Start () {
		Button btn = GetComponent<Button>();
		btn.onClick.AddListener(TaskOnClick);
	}

	void TaskOnClick(){
    SceneManager.LoadScene(SceneManager.GetActiveScene().name)
	}
}
```
