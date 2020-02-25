# QuickTime 設定教學

# 1.有關編碼

Quicktime內建沒有輸出encode功能，

並且螢幕錄製時FPS是浮動的，所以建議如果有在QuickTime有剪輯的動作時記得先用影片轉檔軟體ex:iMovie轉好檔案至mp4之類再給windows，

不然windows播放時會出現影像跟聲音有稍微不同步情況

用iMovie會出現結尾多4秒之類的情況，原因是最後停住後畫面多四秒，但不影響  
但有時要注意結尾有沒有被砍掉

# 2.螢幕錄影沒聲音

[https://github.com/mattingalls/Soundflower/releases](https://github.com/mattingalls/Soundflower/releases)

可以下載Soundflower，然後設定Audio MIDI

> 其他 &gt; Audio MIDI Setup![](/assets/Screen Shot 2018-08-12 at 2.11.27 PM.png)

然後點選左下角的 + ，然後點選Create Multi-output device

> 新增好後記得確認右下U se的順序，讓音源輸出第一個是 Muilt-in Output，這樣電腦才有聲音![](/assets/Screen Shot 2018-08-12 at 2.08.26 PM.png)

然後到 設定 &gt; Audio &gt; Output 設定選擇Multi-Output Device

![](/assets/Screen Shot 2018-08-12 at 2.16.36 PM.png)

然後在QuickTime選擇麥克風來源即可。

![](/assets/Screen Shot 2018-08-12 at 2.09.05 PM 12.png)

> 可參考：[https://blog.allenchou.cc/soundflower/](https://blog.allenchou.cc/soundflower/)

但這樣還不夠，因為我們只能錄螢幕的聲音，還聽不到自己的聲音。

### 所以我們還要設定以下，來同時錄螢幕的聲音與自己麥克風的聲音

[https://zeekmagazine.com/archives/39196](https://zeekmagazine.com/archives/39196)

一樣點選左下的加號，然後這次選擇的是Aggregate Device，然後選擇Soundflower與Built-in Microphone

![](/assets/Screen Shot 2018-08-12 at 2.37.55 PM.png)

然後到 設定 &gt; Audio &gt; Input ，選擇剛才的Aggregate Device

![](/assets/Screen Shot 2018-08-12 at 2.41.14 PM.png)

最後到QuickTime選擇Aggregate Device即大功告成。

> 現在電腦就可以同時收取遊戲的聲音和外面麥克風的聲音了！

![](/assets/Screen Shot 2018-08-12 at 2.36.20 PM.png)

