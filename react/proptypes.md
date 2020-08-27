# propTypes

寫在 React component 內，表示每個 prop 的型別，通常需要搭配 defaultProps

> object 建議使用 shape ·不用 objectOf，不然 react 容易產生錯誤

```javascript
import PropTypes from 'prop-types';

TableRow.defaultProps = {
  item: {},
};

TableRow.propTypes = {
  item: PropTypes.shape({}),
};
```

[https://zh-hant.reactjs.org/docs/typechecking-with-proptypes.html](https://zh-hant.reactjs.org/docs/typechecking-with-proptypes.html)

