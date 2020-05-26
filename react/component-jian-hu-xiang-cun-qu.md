# component 間 互相存取

## Component互相存取

如果不用context API或 redux 時可用以下方法在組建間傳遞prop或method。

1.[https://github.com/kriasoft/react-starter-kit/issues/909\#issuecomment-252969542](https://github.com/kriasoft/react-starter-kit/issues/909#issuecomment-252969542)

```javascript
openDrawer = () => {
  this.drawer.openTheDrawer();
};

<Drawer onRef={ref => (this.drawer = ref)}>
  .....
   <button onClick={() => this.openDrawer()} />
</Drawer>
```

Drawer.js

```javascript
  componentDidMount() {
    this.props.onRef(this)
  }
  openTheDrawer(){
    ....
  };

  render(){
     <div>
        ......
       {this.props.children}
     </div> 
  }
```

這樣就可以存取到parent \(Drawer\) 的function，或是可在parent寫static function，但注意static function 沒有this

## Passing child red to parent

{% embed url="https://stackoverflow.com/questions/42980402/react-passing-ref-from-dumb-componentchild-to-smart-componentparent" %}

現在新的 hook 方法要參考：

[https://stackoverflow.com/questions/37949981/call-child-method-from-parent](https://stackoverflow.com/questions/37949981/call-child-method-from-parent)

