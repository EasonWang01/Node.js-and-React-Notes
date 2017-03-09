用來把經緯度轉換為文字地址

因為此較耗費運算資源，所以google把此api列為超過限制數量後為收費項目


```
            axios.get('https://maps.googleapis.com/maps/api/geocode/json?latlng=' + location.lat() + ',' + location.lng())
              .then((data) => {
                console.log(data)
                data.data.results.forEach((value) => {
                  if(value.types[0] === "administrative_area_level_3" || value.types[0] === "locality" ||value.types[0] === "postal_code") {
                    context.setState({regionName: value.formatted_address});
                  }
                })
                if(!context.state.regionName) {
                  data.data.results.forEach((value) => {
                    if(value.types[0] === "administrative_area_level_2") {
                      context.setState({regionName: value.formatted_address});
                    }
                  })
                }
                if(!context.state.regionName) {
                  data.data.results.forEach((value) => {
                    if(value.types[0] === "administrative_area_level_1") {
                      context.setState({regionName: value.formatted_address});
                    }
                  })
                }
                console.log(context.state.regionName);   
                context.setState({regionName: ''});
              })

```