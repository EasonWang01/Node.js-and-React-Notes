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

### 測試 useHistory, useRouteMatch

[https://stackoverflow.com/questions/58392815/how-to-mock-usehistory-hook-in-jest](https://stackoverflow.com/questions/58392815/how-to-mock-usehistory-hook-in-jest)

```javascript
jest.mock('react-router-dom', () => ({
  useHistory: () => ({
    push: jest.fn(),
  }),
  useRouteMatch: () => ({ url: '' }),
}));
```

## Invariant Violation: You should not use &lt;Route&gt; or withRouter\(\) outside a &lt;Router&gt;

```javascript
import { MemoryRouter as Router } from 'react-router-dom';

// 解法是把他包著Router即可

test('Matching snapshot', () => {
  const { asFragment } = render(
    <Provider store={store}>
      <Router>
        <Auth
          history={{ replace: jest.fn() }}
          location={{ search: '' }}
          signInSuccessUrl=""
          userSignIn={jest.fn()}
          error=""
          login={{ loading: false }}
        />
      </Router>
    </Provider>,
  );
  expect(asFragment()).toMatchSnapshot();
});

```

