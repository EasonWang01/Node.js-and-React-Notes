# 1.無法用e.target

建議採用map並加上index方法傳遞。

https://stackoverflow.com/a/42125039

```js
            {Object.keys(this.state.locationURL).map((location, idx) => (
              <ListItem key={idx} onPress={(e) => this.changeURL(idx)}>
                <Text>{location}</Text>
              </ListItem>
            ))}
```





