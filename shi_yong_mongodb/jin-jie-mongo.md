
操作Mongo的Array

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