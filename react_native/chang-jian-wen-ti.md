# 環境

### 1.![](/assets/螢幕快照 2018-09-03 上午9.51.34.png)Failed to find Build Tools revisui

### 2.

![](/assets/螢幕快照 2018-09-03 上午9.51.43.png)

# 開發

# 1.無法用e.target

建議採用map並加上index方法傳遞。

[https://stackoverflow.com/a/42125039](https://stackoverflow.com/a/42125039)

```js
            {Object.keys(this.state.locationURL).map((location, idx) => (
              <ListItem key={idx} onPress={(e) => this.changeURL(idx)}>
                <Text>{location}</Text>
              </ListItem>
            ))}
```



