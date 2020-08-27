# styled component

{% embed url="https://styled-components.com/" %}

類似於把 style 抽出放在一個 higher order component的概念，但是寫在同一個檔案。

```javascript
import styled from 'styled-components';

const StyledTable = styled.table`
  width: calc(100% - 64px);
  margin: 32px;
  border: 1px solid black;
  background-color: gray;
  thead {
    font-weight: bold;
  }
`

const Table = () => {
  <StyledTable>
    <thead>
      <tr>
  .....
  </StyledTable>
}
```

