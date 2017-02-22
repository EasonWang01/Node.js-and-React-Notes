# #簡介:

為我們輸入地址在input時會自動填入地區


https://developers.google.com/maps/documentation/javascript/places

先到googld console


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


```
var autocomplete = new window.google.maps.places.Autocomplete(this.refs.autosearch);

```


