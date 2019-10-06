# Enzyme

[http://airbnb.io/enzyme/](http://airbnb.io/enzyme/)

提供 React.js 相關的 Render 功能。

完整API文件 :  [https://airbnb.io/enzyme/docs/api/shallow.html](https://airbnb.io/enzyme/docs/api/shallow.html)

> cheatSheet: [https://devhints.io/enzyme](https://devhints.io/enzyme)

## shallow, render, mount 差異

> [https://gist.github.com/fokusferit/e4558d384e4e9cab95d04e5f35d4f913](https://gist.github.com/fokusferit/e4558d384e4e9cab95d04e5f35d4f913)

#### Snapshot testing

```js
import React from 'react';
import { shallow, configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
import test from '../index';

configure({ adapter: new Adapter() });
describe('<test />', () => {
  it('should render and match the snapshot', () => {
    const wrapper = shallow(<test />);
    expect(wrapper).toMatchSnapshot();
  });
});

```

# Jest

類似Mocha，為一個跑測試的執行者

[https://jestjs.io/docs/en/](https://jestjs.io/docs/en/)

#### jest.fn, jest.mock, jest.spyOn 差異

```
jest.fn 可模擬 function 名稱，但邏輯為空，並根據function名字追蹤是否被呼叫
jest.mock 可模擬整個模組內所有function 為 jest.fn()
jest.spyOn 會實際模擬 function 內邏輯
```

#### component Test

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

> 如果是包過compose 的話可以單獨把compoent export 然後傳給 test
>
> ```
> export { Application };
> ```

#### action test

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

#### Reducer test

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

可以打開coverage folder查看他生成的網頁：

![](/assets/Screen Shot 2018-11-23 at 11.33.33 AM.png)上面會清楚的告訴你哪些部分還沒測試到

![](/assets/Screen Shot 2018-11-23 at 11.52.31 AM.png)

通常只要在render的元素加上snapshot與所有事件上 \(onClick...\) 加上測試即可

```js
  test('call getCurrentLocation function', () => {
    const wrapper = shallow(<GoogleMap mapType="pickup" />);
    const getCurrentLocation = jest.spyOn(
      wrapper.instance(),
      'getCurrentLocation',
    );
    const currentLocationButton = wrapper.find('.pickupCurrentRect');
    currentLocationButton.simulate('click');

    wrapper.instance().getCurrentLocation();
    expect(getCurrentLocation).toHaveBeenCalled();
  });
```

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

## 測試 Component Function

```js
import React from 'react';
import { configure, shallow } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
import toJSON from 'enzyme-to-json';
import GoogleMap from './GoogleMap';

const wrapper = shallow(
  <GoogleMap mapType="pickup" pickUpLatitude={1.1} />,
);

test('initMap', () => {
  const spy = jest.spyOn(GoogleMap.prototype, 'initMap');
  wrapper.instance().initMap();
});
```

> 如果是 async function 則 test 也要 async
>
> ```js
>   test('Call handleLocationChange function', async () => {
>     .....
>     await wrapper.instance().handleLocationChange(map);
>   });
> ```

# 測試Module

[https://jestjs.io/docs/en/mock-functions\#mocking-modules](https://jestjs.io/docs/en/mock-functions#mocking-modules)

```js
// users.test.js
import axios from 'axios';

jest.mock('axios');

test('should fetch users', () => {
  const resp = {data: [{name: 'Bob'}]};
  axios.get.mockResolvedValue(resp);
  return Users.all().then(users => expect(users).toEqual(resp.data));
});
```

以及

```js
import * as action from '../booking/action';

jest.mock('../booking/action');
    action.getReverseGeoPOI.mockImplementation(() =>
      Promise.resolve({
        result: [{}],
        extra: [{}],
        trees: [{}],
      }),
    );
```

# 取得 props

```js
// stateless component 用 props() 只能用mount
const wrapper = mount(<GoogleMap mapType="pickup" />);
console.log(wrapper.props())

// class component 用 instance().props
const wrapper = shallow(<GoogleMap mapType="pickup" />);
const props = wrapper.instance().props; // instance 在 react16 後 stateless component都會是null

// 或是取得default prop
const { setPickUpLocation } = GoogleMap.defaultProps;
```

# 測component 不在 class 內的 function

> Export 出來然後於 test 內呼叫即可

# 如果React children component 內的沒cover到

shallow 改為 mount 即可

# 點擊測試

```js
  test('Click openMap', () => {
    wrapper.find(POIMapButton).simulate('click');
  });

  或是

  const item = wrapper.find('[data-testid="listItem"]')
  item.first().simulate('click');
  expect(onSelect).toHaveBeenCalledTimes(1);
```

> find 可以找 id, class 或是 component

# 測試component 內 children的 prop

```js
import Barcharts from '../index';
import { Bar } from 'recharts';

  const wrapper = shallow(
    <Barcharts
      chartData={chartData}
      formatter={tooltipFormatter}
      tooltipFormatter={tooltipFormatter}
      clickable={true}
      dataKey={dataKey}
      onBarClick={onBarClick}
    />,
  );
  wrapper
    .find(Bar)
    .first()
    .props()
    .onClick();
```

# 測試 defaultProp

```js
import Barcharts from '../index';
Barcharts.defaultProps.onBarClick();
```

# Find 元素

```js
    const wrapper = shallow(
      <FilterBlock checkedItem={{ value: 12 }} list={[1, 2]} onSelect={jest.fn()} t={jest.fn()} />,
    );
    console.log(wrapper.find("#test1234").length);


    或是
    <div data-testid="listItem" /> 
    console.log(wrapper.find('[data-testid="listItem"]').length);
```

> 注意:
>
> 1. find\(''\#123"\) or find\(".342"\) 數字開頭的都會出現錯誤
> 2. 不管有沒有成功找到console.log都會顯示 ShallowWrapper {} 所以可以用.length來查看有沒有找到



