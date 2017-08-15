EX:

\(其中的.toMatchSnapshot\(\)會產生一個snapshot檔案，之後如果改動component裡面並造成render結果不同則會產生錯誤需修正\)

\(如果更改檔案但其結果不會影響render結果則不會有錯誤訊息\)

component Test

```js
/**
 * Test the HomePage
 */

import React from 'react';
import {
  shallow,
} from 'enzyme';

import { HomePage, mapDispatchToProps } from '../index';

describe('<HomePage />', () => {
  it('should render correctly', () => {
    const renderedComponent = shallow(
      <HomePage
        lanChange={jest.fn()}
      />,
    );
    expect(renderedComponent).toMatchSnapshot();
  });

  describe('mapDispatchToProps', () => {
    describe('getFolderListAction', () => {
      it('should be injected', () => {
        const dispatch = jest.fn();
        const result = mapDispatchToProps(dispatch);
        expect(result.getFolderListAction).toBeDefined();
      });
    });
  });
});
```

action test

```js
import {
  CLICK_LOGOUT,
} from '../constants';

import {
  clickLogout,
} from '../actions';

describe('Home Actions', () => {
  describe('clickLogout', () => {
    it('should return the correct type', () => {
      const expected = {
        type: CLICK_LOGOUT,
      };

      expect(clickLogout()).toEqual(expected);
    });
  });
});
```

Reducer test

```js
import { fromJS } from 'immutable';

import homeReducer from '../reducer';

describe('homeReducer', () => {
  let state;
  beforeEach(() => {
    state = fromJS({});
  });

  it('should return the initial state', () => {
    const expectedResult = state;
    expect(homeReducer(undefined, {})).toEqual(expectedResult);
  });
});
```





以下先安裝

```
npm install jest-cli -g
```

下面會每次檔案更動都跑test

```
"test:watch": "cross-env NODE_ENV=test jest --watchAll",
```

或是直接輸入jest跑一次test

