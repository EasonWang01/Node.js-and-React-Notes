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

### 測試包含Redux provider 與 store 之 component

```javascript
import React from 'react';
import { Provider } from 'react-redux';
import configureMockStore from 'redux-mock-store';

import { cleanup, render } from '@testing-library/react';

import DataPanel from './index';
const mockStore = configureMockStore();

afterEach(cleanup);
test('Match snapshot', () => {
  const initialState = {
    exportedStudyGroups: {
      loaded: '',
    },
    trainingTasks: {
      target: [],
    },
  };
  const store = mockStore(initialState);
  const { asFragment } = render(
    <Provider store={store}>
      <DataPanel />
    </Provider>,
  );
  expect(asFragment()).toMatchSnapshot();
});

```

