# Shell Script 教學

## 1.暫停秒數

> 注意:單位為秒，非毫秒

```text
sleep 1;
```

## 2.執行指令存為變數

```text
a=$(du -sh ./Images)
echo $a
```

如果裡面有包含多可指令

```text
name=$(basename `pwd`)
echo $name
```

## 3.等於之後不可有空格

```text
a="Hello World!"
echo $a
```

> 如果a = "..."，則會錯誤

## 4.運算要用變數加兩個括號

```text
b=10000
a=2000
a=$(($a + $b))
```

## 5.引入另一個shell script Function

test1.sh

```text
#!/bin/bash
func1() {
   fun=$1
   book=$2
   printf "fun=%s,book=%s\n" "${fun}" "${book}"
};

func2() {
   fun2=$1
   book2=$2
   printf "fun2=%s,book2=%s\n" "${fun2}" "${book2}"
};
```

test2.sh

```text
. ./test1.sh
func1 love horror
func2 ball mystery
```

執行 :

```text
sudo vim test2.sh
```

## 6. function 參數

file.sh

```bash
test() {
  echo $0
  echo $1
  echo $2
}

test 123 234
```

執行 `./file.sh`

回傳 

```text
./file.sh
123
234
```

## 7. for loop

```text
projects=( 
    "test1" 
    "test2" 
)

for i in "${projects[@]}"
do
   echo $i
done
```

> 回傳 
>
> ```text
> test1
> test2
> ```

## grep

> 用來查找字串，記得他匹配到就會把整行回傳

####  用法

```text
grep 關鍵字 檔案1 檔案2 ...
```

查找所有相關檔案

```text
grep words.. /etc/*.conf
```

