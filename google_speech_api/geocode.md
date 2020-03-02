# Geocode

用來把經緯度轉換為文字地址

因為此較耗費運算資源，所以google把此api列為超過限制數量後為收費項目

取得後看到他無法很精準的取得特定你要的區域規則例如縣或市，所以用if來進行判斷，假如沒有找到市就找國家

```text
MapGeocode(location) {
    const context = this;
    axios.get('https://maps.googleapis.com/maps/api/geocode/json?latlng=' + location.lat() + ',' + location.lng())
      .then((data) => {
        console.log(data)
        context.locationVerify(data);  
      })
  }
```

```text
  locationVerify(data) {
     const context = this;
     context.setState({regionName: ''}) //每次點選都把上一次記錄過的地區先清空
      data.data.results.forEach((value) => {
          if(value.types[0] === "administrative_area_level_3" || value.types[0] === "locality" ||value.types[0] === "postal_code") {
            context.setState({regionName: value.formatted_address});
          }
        })
        if(!context.state.regionName) {
          console.log('stage2')
          data.data.results.forEach((value) => {
            if(value.types[0] === "administrative_area_level_2") {
              context.setState({regionName: value.formatted_address});
            }
          })
        }
        if(!context.state.regionName) {
          console.log('stage3')
          data.data.results.forEach((value) => {
            if(value.types[0] === "administrative_area_level_1") {
              context.setState({regionName: value.formatted_address});
            }
          })
        }
  }
```

