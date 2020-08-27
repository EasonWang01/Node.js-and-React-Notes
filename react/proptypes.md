# propTypes

寫在 React component 內，表示每個 prop 的型別，通常需要搭配 defaultProps

```javascript
import PropTypes from 'prop-types';

TableRow.defaultProps = {
  item: {},
};

TableRow.propTypes = {
  item: PropTypes.shape({}),
};
```

{% embed url="https://zh-hant.reactjs.org/docs/typechecking-with-proptypes.html" %}

### Object

有兩種 objectOf 與 shape，PropTypes.objectOf 通常描述裡面的物件型別都相同，而 shape 內會包含多種不同型別。

```javascript
PropTypes.objectOf(PropTypes.number)
PropTypes.shape({ name: PropTypes.string, age: PropTypes.number })
```

[https://stackoverflow.com/a/45764828/4622645](https://stackoverflow.com/a/45764828/4622645)



