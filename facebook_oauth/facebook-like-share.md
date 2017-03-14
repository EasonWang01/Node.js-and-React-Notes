#facebook 按讚與分享API


按讚

給不同的`data-href`即可區別不同的讚

```
<div className="fb-like" data-href='https://sakatu.com/123' data-layout="standard" data-action="like" data-size="small" data-show-faces="true" ></div>

<div className="fb-like" data-href='https://sakatu.com/1232' data-layout="standard" data-action="like" data-size="small" data-show-faces="true" ></div>

```


分享

可以直接嵌入或是使用API  `FB.ui`方式呼叫

以下示範`FB.ui`

https://developers.facebook.com/docs/sharing/reference/share-dialog
```
   FB.ui(
    {
        method: 'share',
        href: 'your_url',     // The same than link in feed method
        title: 'your_title',  // The same than name in feed method
        picture: 'path_to_your_picture',  
        caption: 'your_caption',  
        description: 'your_description',
     },
     function(response){
        // your code to manage the response
     });
```