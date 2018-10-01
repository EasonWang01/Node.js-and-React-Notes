# Component互相存取

如果不用context API或 redux 時可用以下方法在組建間傳遞prop或method。

1.[https://github.com/kriasoft/react-starter-kit/issues/909\#issuecomment-252969542](https://github.com/kriasoft/react-starter-kit/issues/909#issuecomment-252969542)

```js
openDrawer = () => {
  this.drawer.openTheDrawer();
};

<Drawer onRef={ref => (this.drawer = ref)}>
  .....
   <button onClick={() => this.openDrawer()} />
</Drawer>
```

Drawer.js

```js
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

