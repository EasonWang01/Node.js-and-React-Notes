# Handlebars.js

存取陣列
```
[{ 
  title: "My New Post", 
  body: "This is my first post!"
}]

{{#each this}}
 {{title}}
{{/each}}
```
有index的陣列
```
{
article:[{ 
  title: "My New Post", 
  body: "This is my first post!"
}]
}

{{#each article}}
 {{title}}
{{/each}}


```
存取物件
```
 
{{#with this}}
 {{title}}
{{/with}}


{ 
  title: "My New Post", 
  body: "This is my first post!"
}
```