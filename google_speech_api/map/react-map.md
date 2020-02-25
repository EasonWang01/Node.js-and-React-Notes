# React 與 Google MAP

可以使用如下用法：

```js
  componentDidMount() {
    const googleMapURL: `https://maps.googleapis.com/maps/api/js?key=${API_KEY}`,
    if (!window.google) {
      const scriptjs = require(`scriptjs`);
      scriptjs(googleMapURL, () => this.initMap());
    } else {
      this.initMap();
    }
  }

  initMap() {
    const google = window.google;
    const map = new google.maps.Map(
      document.getElementById(填入在DOM上的一個空的 div ID), // 初始化地圖
      {
        zoom: 15,
        center: { lat: 1.2948257152432294, lng: 103.85845150268554 },
        fullscreenControl: false, // 隱藏預設的地圖按鈕
        streetViewControl: false,  // 隱藏預設的地圖按鈕
        mapTypeControlOptions: {
          mapTypeIds: [],
        },
      },
    );
    window.map = map;
  }
```

# 客製化地圖

在地圖左上放入元素：

```js
    map.controls[google.maps.ControlPosition.LEFT_TOP].push(
      document.getElementById('dropoffLegend'),
    );
```

加上預設圖型 Marker

```js
      const marker = new google.maps.Marker({
        position: { lat, lng },
        map,
        title: spot.address.name,
        icon: {
          path: google.maps.SymbolPath.CIRCLE,
          fillOpacity: 1.0,
          fillColor: '#33c072',
          strokeOpacity: 1,
          strokeColor: 'white',
          strokeWeight: 2.0,
          scale: 9.0,
        },
      });
```

加上地圖事件

```js
google.maps.event.addListener(map, 'dragstart', () => {
  this.setState({ dragging: true });
});
```

如果想要在地圖上加入中心點，並且保持滑動後永遠中心，可以加上一個絕對定位元素。

```js
 .centerMarker {
   position: absolute;
   width: 80px;
   top: ~"calc(50% - 70px)";
   left: 0;
   right: 0;
   margin: 0 auto;
 }
```

```html
        <div
          style={{
            position: 'relative',
            display: 'inline-block',
            width: '100%',
          }}
        >
          <div className={styles.map} id={this.props.mapType} />
          <div className="centerMarker">
       </div>
```

> 跟地圖同層，然後父層是一個 `relative `元素



