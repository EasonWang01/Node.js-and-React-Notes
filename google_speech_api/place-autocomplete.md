# #簡介:

為我們輸入地址在input時會自動填入地區


https://developers.google.com/maps/documentation/javascript/places

中文
https://developers.google.com/places/web-service/intro

先到google console 新增相關api與key


# #使用

####1.申請ＡＰＩ服務

要申請兩個

一個是Google Maps JavaScript API
另一個是Google Places API Web Service

因為place需求Maps API


####2.載入SDK並更改api key

```
<script type="text/javascript" src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDAodeKstiifB9WhEnZrZY7_mA8abcyJpY&libraries=places"></script>
```

####3.啟用

```
<input ref="autosearch" type="text" size="50" />
```

>因使用React所以google前面加上window

```
var autocomplete = new window.google.maps.places.Autocomplete(this.refs.autosearch);

```


####4.偵測輸入

1.用了除與2是因為他有兩個數值(目前不清楚f,b分別表示什麼)
2.可使用geocode來把經緯度轉換為地址(但geocode api官方表示耗用資源高，所以每天有限制免費的request數目，超過將收費)

```javascript
    window.google.maps.event.addListener(autocomplete, 'place_changed', function () {
        var place = autocomplete.getPlace();
        console.log(place)
        if(typeof place.geometry.viewport === 'undefined') {
          alert('您好，此位置無法識別請輸入其他地址，謝謝');
          context.refs.autosearch.value = '';
          return
        }
        if (place.geometry.viewport) {
          const lat = (place.geometry.viewport.f.b + place.geometry.viewport.f.f) / 2;
          const lng = (place.geometry.viewport.b.b + place.geometry.viewport.b.f) / 2;
          axios.get('https://maps.googleapis.com/maps/api/geocode/json?latlng=' + lat + ',' + lng)
          .then((data) => {
                let placeInfo = {
                  address: place.formatted_address,
                  minAddress: data.data.results[data.data.results.length - 4].formatted_address, 
                  lat: lat,
                  lng: lng
                }
                context.setState({place: placeInfo});
                console.log(placeInfo)
          });
        }
    });
  }
```

簡單範例
http://stackoverflow.com/questions/18345875/google-places-api-example

