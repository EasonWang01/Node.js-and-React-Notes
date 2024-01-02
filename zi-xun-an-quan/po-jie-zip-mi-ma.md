---
description: 有時我們忘記當時設定的密碼，可以如下來找回
---

# 破解 ZIP 密碼

以下使用 Mac 的 OSX 系統，安裝 john the ripper。

[https://openwall.info/wiki/john/johnny](https://openwall.info/wiki/john/johnny)

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

## 其他資料

1.  **字典攻擊 (Dictionary Attack)**:

    使用字典攻擊需要提供字典文件，以下是示例指令：

    ```bash
    john --wordlist=dictionary.txt hashes.txt
    ```

    * `dictionary.txt` 是包含要嘗試的單詞或密碼的字典文件。
    * `hashes.txt` 是包含密碼雜湊的文件。
2.  **暴力攻擊 (Brute Force Attack)**:

    暴力攻擊通常需要設定字符集、最小長度和最大長度，以下是示例指令：

    ```bash
    john --incremental --min-length=6 --max-length=8 hashes.txt
    ```

    * `--incremental` 表示使用暴力攻擊模式。
    * `--min-length` 和 `--max-length` 是密碼長度的範圍。
3.  **混合攻擊 (Hybrid Attack)**:

    混合攻擊結合了字典攻擊和暴力攻擊，以下是示例指令：

    ```bash
    john --wordlist=dictionary.txt --rules --min-length=6 --max-length=8 hashes.txt
    ```

    * `--wordlist` 指定字典文件。
    * `--rules` 啟用規則引擎，用於生成可能的密碼組合。
4.  **彩虹表攻擊 (Rainbow Table Attack)**:

    彩虹表攻擊需要提供彩虹表文件，以下是示例指令：

    ```bash
    john --format=nt --pot=cracked.txt --fork=4 hashes.txt
    ```

    * `--format` 指定雜湊格式（根據需要修改）。
    * `--pot` 指定已破解的密碼將被儲存的文件。
    * `--fork` 啟用多進程，提高破解效率。
5.  **應用特定攻擊 (Application-Specific Attack)**:

    應用特定攻擊需要根據目標應用程序的特定特徵進行設置，並可能需要使用自定義的規則和字典。
6.  **暴力攻擊 (Rule-Based Attack)**:

    暴力攻擊需要提供一個規則文件，以下是示例指令：

    ```bash
    john --wordlist=dictionary.txt --rules=myrules.txt hashes.txt
    ```

    * `--rules` 指定規則文件，其中包含自定義的密碼生成規則。
