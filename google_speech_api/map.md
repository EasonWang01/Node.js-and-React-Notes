# #在網站放入Google的地圖

放入元素，把地圖置中(記得設定寬高)
```html
<div style={{height: '300px',width: '300px', marginLeft: 'auto', marginRight: 'auto'}} id="map"></div>

```

初始化
```javascript
      var map;
      var marker;

      function initMap() {
        map = new window.google.maps.Map(document.getElementById('map'), {
          center: {lat: 23.594194326203837, lng: 120.970458984375},
          zoom: 7
        });
      }
      initMap();

      window.google.maps.event.addListener(map, 'click', function(event) {
        placeMarker(event.latLng);
      });
      
      
      
      
       function placeMarker(location) {
         if (marker == null) {
            marker = new window.google.maps.Marker({
                position: location,
                map: map
            }); 
          } else {
                marker.setPosition(location); 

          };
       
       }
```