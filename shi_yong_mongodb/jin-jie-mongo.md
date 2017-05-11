
#操作Mongo的Array

https://docs.mongodb.com/manual/reference/operator/update/pull/#up._S_pull


Ex: 以下可把rating_my Array中的item物件中的_id與req.body.item._id
值相同的所有欄位移除
```

    User.update({_id: req.token.data._id}, {$pull: {  //先把舊的移除
      rating_my: {
        "item._id": req.body.item._id
      }
    }})
    
```    


#分頁快速query

http://stackoverflow.com/questions/7228169/slow-pagination-over-tons-of-records-in-mongo


# 使用Geo seearch

https://docs.mongodb.com/manual/reference/operator/query-geospatial/

以下使用$near為範例

1.先在schema建立index
```
var c = new mongoose.Schema({
    status: String,   //物品承租階段狀態 eg,尋租中,已出租,已還租
    rented: Boolean, //是否已出租
    renterInfo: Object, //承租者相關資訊，填表
    lessee: String,  //承租者
    geometry: {
        type: {
            type: String,
            default: "Point"
        },
        coordinates: [Number]
    }, //物品所在地
    regionName: String,

})

c.index({geometry: '2dsphere'});

exports.Test = mongoose.model('test', c)
```

建立後可以查看到index
![](/assets/螢幕快照 2017-05-11 下午5.44.50.png)

2.之後即可搜尋

```
    Test.find({geometry:
      { $near :
        {
          $geometry: {type : "Point", coordinates: [ -149.0133, 64.7440 ] },
          $minDistance: 0,
          $maxDistance: 50
        }
      }})
    .then(data => {
      //console.log(data)
      res.end(JSON.stringify(data))
    })
    .catch(err => {
      console.log(err);
      res.end(err.toString());
    })
```
