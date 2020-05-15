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

### Mock 某個 import 的 module

```javascript
jest.mock('axios', () => ({
  post: () => ({
    ....
  })
}));
```

## Uncaught \[Invariant Violation: Element type is invalid: expected a string \(for built-in components\) or a class/function \(for composite components\) but got: undefined.

有可能是包了 router 在 component外，然後又 mock 了 react-router-dom。

## 傳入 i18n 

已 i18n-next 為例

```javascript
import { Translation } from 'react-i18next';
import i18n from '@/i18n'; // i18n init file

<Translation ns="agreements" i18n={i18n}>
  {t => (
     <..Component..
       t={t}
     />
  )}
</Translation>
```

## 傳入 Material-UI theme

```javascript
import { createMuiTheme, MuiThemeProvider } from '@material-ui/core/styles';
import { ThemeProvider } from '@material-ui/styles';
import theme from '@/constants/theme'; 
const muiTheme = createMuiTheme(theme);

test('Matching snapshot', () => {
  const { asFragment } = render(
    <MuiThemeProvider theme={muiTheme}>
      <ThemeProvider theme={muiTheme}>
        <Provider store={store}>
          <StatisticsChart data={[{}, {}]} theme={muiTheme} />
        </Provider>
      </ThemeProvider>
    </MuiThemeProvider>,
  );
  expect(asFragment()).toMatchSnapshot();
});

```

