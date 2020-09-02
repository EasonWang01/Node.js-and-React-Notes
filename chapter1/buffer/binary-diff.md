# Binary Diff

有幾種方式可以查看 binary 檔案的 diff

## xdelta

1.下載release v3.1.0，有些新版本編譯會有問題

[https://github.com/jmacd/xdelta/releases](https://github.com/jmacd/xdelta/releases)

2.編譯

```text
cd ~/xdelta-3.1.0/xdelta3
brew install automake
autoreconf --install
./configure
make
```

3.執行

```text
./xdelta3 --help
```



