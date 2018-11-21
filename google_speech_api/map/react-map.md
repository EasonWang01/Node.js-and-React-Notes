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
      document.getElementById(this.props.mapType), // 初始化地圖
      {
        zoom: 15,
        center: { lat: 1.2948257152432294, lng: 103.85845150268554 },
        fullscreenControl: false,
        streetViewControl: false,
        mapTypeControlOptions: {
          mapTypeIds: [],
        },
      },
    );
    window.map = map;
  }
```



