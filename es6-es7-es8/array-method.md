# Array method

## Sort

```javascript
const arr = [2, 5, 3];
arr.sort((a, b) => a - b);
// [2, 3, 5]
```

會改變原來的 Array，如果不想改變到原本 Array 可用以下：

```javascript
const sorted = [...arr].sort();
// 或是
const sorted = arr.slice(0).sort();
```

[https://stackoverflow.com/questions/9592740/how-can-you-sort-an-array-without-mutating-the-original-array](https://stackoverflow.com/questions/9592740/how-can-you-sort-an-array-without-mutating-the-original-array)

## Map

## Reduce

## Filter

## Some

## Find

