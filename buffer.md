# Buffer

#### \# 先備知識

1. BE LE 分別代表big endian 與small endian\(大的數至小的數要從右或從左開始排列\)

   EX:

```
Data=0x12345678    

Big Endian的系統：
存到記憶體會變成 0x12 0x34 0x56 0x78，最高位元組在位址最低位元，最低位元組在位址最高位元，依次排列

Little Endian的系統：
存到記憶體會變成 0x78 0x56 0x34 0x12，最低位元組在最低位元，最高位元組在最高位元，反序排列
```

# \#操作

buf會被分配隨機buffer

```
buf = new Buffer(6)
```

我們自己初始化buffer

```
buf = new Buffer([0,0,0,0,0,0])

<Buffer 00 00 00 00 00 00>
```

寫資料\(0到6為offset\)

```
 buf.writeUIntBE(12345, 0 , 6)

 回傳為<Buffer 00 00 00 00 30 39>
```

> 3039為16進位，轉為10進位就是12345

試著改看看offset

```
buf.writeUIntBE(12345, 0 , 2)

<Buffer 30 39 00 00 00 00>


buf.writeUIntBE(12345, 0 , 4)

<Buffer 00 00 30 39 00 00>
```

## 注意:buffer如重複write則原本其他offset的數字還是會存在

發現offset後面的數字先減2後的數字即為前面是00的長度

然後後面不足部分會自動重複最後一個buffer數字\(這裡後面重複39\)

> 根據官方說明:Where to start reading. Must satisfy:`0 <= offset <= buf.length - 4`

所以如果輸入以下會產生錯誤buf.writeUIntBE\(12345, 0 , 1\)，因為6位的buffer-4後至少要offset大於等於2

#### \#讀取

試試看以下

```
buf = Buffer.from([0, 0, 0, 5]);
<Buffer 00 00 00 05>


> buf.readInt32BE(3,4).toString(16)
'5000000'
> buf.readInt32BE(2,4).toString(16)
'50000'
> buf.readInt32BE(1,4).toString(16)
'500'
> buf.readInt32BE(0,4).toString(16)
'5'
> buf.readInt32BE(4,4).toString(16)
'0'
```

> UInt 無號整數的型態名稱為 uint8、uint16、uint32、uint64，顧名思義，使用的長度分別為 8 位元、16 位元、32 位元與 64 位元。
>
> 舉例來說，uint8 可儲存的整數範圍為 0 到 255 ，共256個數   因為2\*\*8 \(二的八次方為256\)

## writeUInt8

> write如果第一個參數前面沒加0x 他會以為你填入的是十進位來看

```js
var buffLen = 128;

var buffer = new Buffer(buffLen);
for (var i = 0; i < buffLen; i += 1) {
    buffer.writeUInt8(1, i);
}

console.log(buffer)
```

## writeUInt16BE

\(除了8以外其他後面都需要加上BE或LE，包含16和32（沒有64以及以上）\)

因為buffer是以16進位表示，所以16BE的意思是16bits的Big endian，一共是4個字 因為\(2\*\*4 = 16\)

```js
var buffer = new Buffer(buffLen);
for (var i = 0; i < buffLen; i += 2) { // 因為buffer一次寫入會寫入2個bits，所以Int16的話共4個bits所以換下一個時要i += 2
    buffer.writeUInt16LE(0x0102, i); // 如果改為buffer.writeUInt16LE(0x010203, i); 會出現outbound錯誤
}

console.log(buffer)
```

## writeUInt32BE





# 關於[NodeJS Buffer to JavaScript ArrayBuffer](https://stackoverflow.com/questions/8609289/convert-a-binary-nodejs-buffer-to-javascript-arraybuffer) 的轉換可參考

https://stackoverflow.com/questions/8609289/convert-a-binary-nodejs-buffer-to-javascript-arraybuffer

  




