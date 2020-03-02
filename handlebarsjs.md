# Handlebars.js

存取陣列

```text
[{ 
  title: "My New Post", 
  body: "This is my first post!"
}]

{{#each this}}
 {{title}}
{{/each}}
```

有index的陣列

```text
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

```text
{{#with this}}
 {{title}}
{{/with}}


{ 
  title: "My New Post", 
  body: "This is my first post!"
}
```

