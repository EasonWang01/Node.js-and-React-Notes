# 熱力圖 heatmap

### 安裝:

```javascript
yarn add heatmap.js
```

> [https://gist.github.com/cristiandley/46157c292a97c99986f8bd98fcf89d0d](https://gist.github.com/cristiandley/46157c292a97c99986f8bd98fcf89d0d)[https://www.patrick-wied.at/static/heatmapjs/](https://www.patrick-wied.at/static/heatmapjs/) 原先的package較舊，所以用別人打包成npm的版本

### 使用:

```javascript
import h337 from 'heatmap.js';
```

新增兩個div，一個加上absolute，一個用來顯示canvas

```
<div className={styles.heatmapCanvasContainer}>
  <div className={styles.heatmapCanvas} ref={heatMapEle}></div>
</div>
```

```css
.heatmapCanvasContainer {
  position: absolute;
  width: 768px;
  height: 768px;
}

.heatmapCanvas {
  width: 100%;
  height: 100%;
}
```

加上heatmap data

```javascript
h337.create({
  container: heatMapEle.current,
 radius: 1,
}).setData({
  min: 0,
  max: 1,
  data
});
```

> data 格式：` [{x: <num>, y: <num>, value: <num>}, ....]`

清除：

```javascript
    const clearHeatMap = () => {
      heatMapEle.current.innerHTML = "";
    }
```
