# Windos 相關

用 Mac 製作 win 11 bootable USB

> 因為 FAT32 格式才能製作開機碟，但 FAT32 限制單一檔案大小為 4GB，而 win11 的檔案為 5GB，
>
> 所以需要用 wimlib 拆分檔案

1.下載 window 11 ISO file

[https://www.microsoft.com/zh-tw/software-download/windows11](https://www.microsoft.com/zh-tw/software-download/windows11)

2\. mount IOS 檔案到電腦，格式化 USB 為 FAT32

```
hdiutil mount ~/Downloads/Win11_22H2_Chinese_Traditional_x64v1.iso
```

3\. 用 Rsync 指令複製檔案到 USB, 但不要複製到 `sources/install.wim`

```
rsync -vha –exclude=sources/install.wim /Volumes/CCCOMA_X64FRE_ZH-TW_DV9/* /Volumes/32G-USB
```

4\. 用 winlib 切分 install.win 然後單獨複製過去

```
brew install wimlib
wimlib-imagex split /Volumes/CCCOMA_X64FRE_ZH-TW_DV9/sources/install.wim /Volumes/32G-USB/sources/install.swm 3000
```

![](<.gitbook/assets/截圖 2023-01-20 上午10.41.56.png>)

[https://learn.microsoft.com/en-us/answers/questions/741412/how-to-create-a-windows-11-bootable-usb-installati](https://learn.microsoft.com/en-us/answers/questions/741412/how-to-create-a-windows-11-bootable-usb-installati)
