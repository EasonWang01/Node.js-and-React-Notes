# path


##path.join  用於連結路徑

開啟cmd輸入node，再輸入
```
path.join("aa", "foo");
```
Unix系统是”/“，Windows系统是”\“。所以用path.join可正確連結


##path.resolve 返回絕對路徑

一樣在REPL輸入
```
path.resolve('foo/bar', '/tmp/file/')
```

##path.relative 返回兩個參數之間的相對路徑

```
path.relative('/data/apple', '/data/banana')
```