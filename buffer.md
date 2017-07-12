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



buf會被分配隨機buffer

```
buf = new Buffer(6)
```

寫資料\(0到6為offset\)

```
 buf.writeUIntBE(12345, 0 , 6)
 
 回傳為<Buffer 00 00 00 00 30 39>
```

> 3039為16進位，轉為10進位就是12345

試著改看看offset

```
buf.writeUIntBE(12345, 0 , 5)

<Buffer 00 00 00 30 39 39>

 buf.writeUIntBE(12345, 0 , 4)

<Buffer 00 00 30 39 39 39>
```

發現offset後面的數字先減2後的數字即為前面是00的長度

然後後面不足部分會自動重複最後一個buffer數字\(這裡後面重複39\)

> 根據官方說明:



