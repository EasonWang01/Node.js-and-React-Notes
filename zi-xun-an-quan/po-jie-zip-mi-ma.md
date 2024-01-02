---
description: 有時我們忘記當時設定的密碼，可以如下來找回
---

# 破解 ZIP 密碼

以下使用 Mac 的 OSX 系統。

1. 因為 mac 無法直接點選產生加密的 zip，所以使用以下產生範例 ZIP

```
zip -er test.zip '/Users/test.png'
```

2. 安裝 john the ripper

```
brew install john-jumbo
```

3. 查看可用工具

```
cd /usr/local/Cellar/john-jumbo/1.9.0_1/share/john
```

> 上面版本 /1.9.0\_1 可能會改變，看你安裝的版本

4. 產生 john 檔案

```
./zip2john ~/test.zip > ~/testzip.john
```

5. 開始跑破解程式

```
john --incremental ~/testzip.john
```

6. 查看破解密碼

```
john --show ~/testzip.john
```

> 會出現在檔案名稱後面的：後面
