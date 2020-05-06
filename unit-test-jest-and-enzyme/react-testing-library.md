---
description: 'https://testing-library.com/docs'
---

# React Testing Library

目前被稱作比enzyme更好用的測試工具。

### 測試 snapshot

```javascript
import { render } from '@testing-library/react';

test('Match snapshot', () => {
  const { asFragment, getByTestId } = render(
    <Agreement t={jest.fn()} />
  );
  expect(asFragment()).toMatchSnapshot();
});
```

