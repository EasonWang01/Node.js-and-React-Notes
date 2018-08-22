# 作法

1.因為React的jsx無法被模組化直接使用，所以要先將jsx轉為js

2.export default 等以及import也無法使用

因為node\_module被使用時不會被compile

> css 不用 compile 也可以，因為通常專案在compile 後的 jsx 有 require css，這時會被專案的 webpack loader compile。

# 使用babel compile

```
babel src -d dist --presets es2015,stage-2,react --copy-files
```

可參考此Repo  
[https://github.com/EasonWang01/react-pure-css-modal](https://github.com/EasonWang01/react-pure-css-modal)

或是

線上compile

[https://facebook.github.io/react/jsx-compiler.html](https://facebook.github.io/react/jsx-compiler.html)

[https://babeljs.io/repl/](https://babeljs.io/repl/)

EX:

```
class Modal extends Component {
  render() {
    return (
      <div>
          <input className="yicheng-modal-state" id={props.id} type="checkbox" />  
          <div className="yicheng-modalbg" style={{width: '100%', height: '100%', background: 'rgba(0,0,0, .6)', position: 'fixed', top: '0', left: '0'}}>
            <div className="yicheng-modalWhite" style={Object.assign({zIndex:'100',width: '50%', height: '50%', background: 'white', position: 'fixed', top: '25%', left: '25%', overflowY: 'scroll'},props.style)} >
            {props.children}
            </div> 
            <label onClick={() => props.onClose()} className="yicheng-modal__bg" htmlFor={props.id}></label>
          </div>     
      </div>
    )
  }
}
```

compile 後

```
var _createClass = function () { function defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } } return function (Constructor, protoProps, staticProps) { if (protoProps) defineProperties(Constructor.prototype, protoProps); if (staticProps) defineProperties(Constructor, staticProps); return Constructor; }; }();

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

function _possibleConstructorReturn(self, call) { if (!self) { throw new ReferenceError("this hasn't been initialised - super() hasn't been called"); } return call && (typeof call === "object" || typeof call === "function") ? call : self; }

function _inherits(subClass, superClass) { if (typeof superClass !== "function" && superClass !== null) { throw new TypeError("Super expression must either be null or a function, not " + typeof superClass); } subClass.prototype = Object.create(superClass && superClass.prototype, { constructor: { value: subClass, enumerable: false, writable: true, configurable: true } }); if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass; }

var Modal = function (_Component) {
  _inherits(Modal, _Component);

  function Modal() {
    _classCallCheck(this, Modal);

    return _possibleConstructorReturn(this, (Modal.__proto__ || Object.getPrototypeOf(Modal)).apply(this, arguments));
  }

  _createClass(Modal, [{
    key: "render",
    value: function render() {
      return React.createElement(
        "div",
        null,
        React.createElement("input", { className: "yicheng-modal-state", id: props.id, type: "checkbox" }),
        React.createElement(
          "div",
          { className: "yicheng-modalbg", style: { width: '100%', height: '100%', background: 'rgba(0,0,0, .6)', position: 'fixed', top: '0', left: '0' } },
          React.createElement(
            "div",
            { className: "yicheng-modalWhite", style: Object.assign({ zIndex: '100', width: '50%', height: '50%', background: 'white', position: 'fixed', top: '25%', left: '25%', overflowY: 'scroll' }, props.style) },
            props.children
          ),
          React.createElement("label", { onClick: function onClick() {
              return props.onClose();
            }, className: "yicheng-modal__bg", htmlFor: props.id })
        )
      );
    }
  }]);

  return Modal;
}(Component);
```

# 測試元件

1.把元件放到create-react-app 的 src folder測試

