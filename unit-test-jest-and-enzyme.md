# Enzyme

[http://airbnb.io/enzyme/](http://airbnb.io/enzyme/)

僅提供React.js相關的Render功能。

包含: shallow, render, mount等等

[https://gist.github.com/fokusferit/e4558d384e4e9cab95d04e5f35d4f913](https://gist.github.com/fokusferit/e4558d384e4e9cab95d04e5f35d4f913)

> cheatSheet: [https://devhints.io/enzyme](https://devhints.io/enzyme)

# Jest

類似Mocha，為一個跑測試的執行者

[https://jestjs.io/docs/en/](https://jestjs.io/docs/en/)

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

> 如果沒有過所有測試，則不會產生_snapshots_

## 也可用react-test-renderer

```js
import React from 'react';
import toJSON from 'enzyme-to-json';
import renderer from 'react-test-renderer';
import SectionDisplay from './SectionDisplay.jsx';

describe('SectionDisplay component', () => {
  test('renders without crashing', () => {
    const wrapper = renderer.create(<SectionDisplay />);
    expect(toJSON(wrapper)).toMatchSnapshot();
  });
});
```

# Test coverage

```
--coverage
```

使用如上指令會顯示目前測試覆蓋率。

通常只要在render的元素加上snapshot與所有事件上 \(onClick...\) 加上測試即可

```js
import React from 'react';
import { configure, mount } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
import toJSON from 'enzyme-to-json';
import BookingDetail from './BookingDetail';
import SectionDisplay from '../common/SectionDisplay';
import BookingStatus from '../history/BookingStatus';
import ModalDisplay from '../common/ModalDisplay';

configure({ adapter: new Adapter() });

describe('bookingDetail', () => {
  const clickCancelModal = jest.fn();
  const wrapper = mount(
    <BookingDetail clickCancelModal={clickCancelModal} showCancelModal />,
  );
  test('renders without crashing', () => {
    expect(toJSON(wrapper)).toMatchSnapshot();
  });
  test('Render SectionDisplay inside bookingDetail', () => {
    expect(wrapper.find(SectionDisplay).length).toBe(3);
  });
  test('Render BookingStatus inside bookingDetail', () => {
    expect(wrapper.find(BookingStatus).exists());
  });
  test('Render ModalDisplay inside bookingDetail', () => {
    expect(wrapper.find(ModalDisplay).exists());
  });
  test('Render ModalDisplay inside bookingDetail', () => {
    expect(wrapper.props().showCancelModal).toBe(true);
  });
  test('Click cancel modal Button', () => {
    wrapper
      .find('Button')
      .first()
      .simulate('click');
    expect(clickCancelModal).toHaveBeenCalled();
  });
  test('Click back btn in modal', () => {
    wrapper
      .find('Button')
      .at(1)
      .simulate('click');
    expect(clickCancelModal).toHaveBeenCalled();
  });
  test('Click cancelBooking btn Button', () => {
    wrapper
      .find('Button')
      .at(2)
      .simulate('click');
    expect(clickCancelModal).toHaveBeenCalled();
  });
});
```

> 如果沒有在所有Onclick事件加上測試，則會無法有100%覆蓋。

## 測試Function

```js
describe('ToLocalStorage', () => {
    const localStorageMock = {
      getItem: jest.fn(),
      setItem: jest.fn(),
      clear: jest.fn(),
    };
    global.localStorage = localStorageMock;
    beforeEach(() => {
      localStorage.setItem.mockClear();
      localStorage.getItem.mockClear();
    });
    test('should save location to localStorage', () => {
      savingLocation({});
      expect(localStorage.setItem).toHaveBeenCalled();
    });
    test('should get location from localStorage', () => {
      getPastLocation({});
      expect(localStorage.getItem).toHaveBeenCalled();
    });
  });
```

> Reset先前的function call可用
>
> ```
> beforeEach(() => {
>   localStorage.setItem.mockClear();
>   localStorage.getItem.mockClear();
> });
> ```



