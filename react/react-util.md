# React util

### React menu 

採用遞迴做法把所有子清單都顯示出來

config.js

```javascript
export default [
  {
    label: 'Foobar',
    children: [
      { label: 'Foo', parentLabel: 'Foobar' },
      { label: 'Bar', parentLabel: 'Foobar' },
    ],
  },
  {
    label: 'Joedoe',
    children: [
      {
        label: 'Joe',
        children: [{ label: 'Hello' }],
      },
      {
        label: 'Doe',
        children: [{ label: 'world!' }],
      },
    ],
  },
  { label: 'Logout' },
];
```

index.js

```javascript
import React, { useState } from 'react';
import SubHeaderMenu from './SubHeaderMenu';
import menus from './config';

const SubHeader = () => {
  const [showSubHeader, setShowSubHeader] = useState(false);
  return (
    <div
      style={{ display: 'inline-flex' }}
      onFocus={() => setShowSubHeader(true)}
      onMouseOver={() => setShowSubHeader(true)}
    >
      <h1>Menus</h1>
      <div style={{ marginTop: '30px' }}>
        {showSubHeader && <SubHeaderMenu menus={menus} />}
      </div>
    </div>
  );
};

export default SubHeader;
```

SubHeaderMenu.js

```javascript
import React, { useState } from 'react';
import styled from 'styled-components';
import PropTypes from 'prop-types';

const Menu = styled.div`
  border: 1px solid grey;
  background-color: white;
  width: 200px;
  padding: 8px;
  overflow-y: hidden;
`;

const SubHeaderMenu = ({ menus }) => {
  const [showSubHeader, setShowSubHeader] = useState(false);
  const [nextChildren, setNextChildren] = useState({});
  const handleMenuClick = (e, label) => {
    e.stopPropagation();
    // eslint-disable-next-line no-alert
    alert(`Clicked at ${label}`);
  };
  const handleMouseOver = (children) => {
    setShowSubHeader(true);
    setNextChildren(children);
  };
  const handleMouseOut = () => {
    // TODO
    // setShowSubHeader(false);
    // setNextChildren({});
  };
  return (
    <div style={{ display: 'flex' }}>
      <div>
        {menus.map(({ label, children }) => (
          <div
            role='menuitem'
            tabIndex={0}
            key={label}
            onClick={(e) => handleMenuClick(e, label)}
            onKeyPress={(e) => handleMenuClick(e, label)}
            onMouseOut={handleMouseOut}
            onBlur={handleMouseOut}
            style={{ display: 'flex' }}
          >
            <Menu>
              {label}
              <span
                onMouseOver={() => handleMouseOver(children)}
                onFocus={() => handleMouseOver(children)}
                style={{ float: 'right' }}
              >
                {children ? '>' : ''}
              </span>
            </Menu>
          </div>
        ))}
      </div>
      {showSubHeader && <SubHeaderMenu menus={nextChildren} />}
    </div>
  );
};

export default SubHeaderMenu;

SubHeaderMenu.defaultProps = {
  menus: [{}]
}

SubHeaderMenu.propTypes = {
  menus: PropTypes.arrayOf(PropTypes.object)
}
```

![](../.gitbook/assets/ying-mu-kuai-zhao-20200827-xia-wu-3.31.38.png)

## React util

### 1.如有一個共用的Component

不要於component的style 寫上如

```text
Object.assign(props.style,style.item)
```

這樣共用元素的頁面會互相覆蓋style  
因為react不會重新render元素

### 2.使用Rich Editor

這裡使用Draft.js 做example

官方的usage只給了一般的editor

如要有上方按鈕需使用官方提供的richEditor範例

[https://github.com/facebook/draft-js/blob/master/examples/rich/rich.html](https://github.com/facebook/draft-js/blob/master/examples/rich/rich.html)

我們使用時會先把它包成一個component在做引入

1.先新增RichEditor.js

```text
import React, { Component, PropTypes as T } from 'react';
import { connect } from 'react-redux';
import radium from 'radium';
import { Editor, EditorState, RichUtils } from 'draft-js';

class RichEditor extends Component {



  constructor(props) {
    super(props);
    this.state = { editorState: EditorState.createEmpty() };

    this.focus = () => this.refs.editor.focus();
    this.onChange = (editorState) => this.setState({ editorState });

    this.handleKeyCommand = (command) => this._handleKeyCommand(command);
    this.onTab = (e) => this._onTab(e);
    this.toggleBlockType = (type) => this._toggleBlockType(type);
    this.toggleInlineStyle = (style) => this._toggleInlineStyle(style);
  }

  _handleKeyCommand(command) {
    const { editorState } = this.state;
    const newState = RichUtils.handleKeyCommand(editorState, command);
    if (newState) {
      this.onChange(newState);
      return true;
    }
    return false;
  }

  _onTab(e) {
    const maxDepth = 4;
    this.onChange(RichUtils.onTab(e, this.state.editorState, maxDepth));
  }

  _toggleBlockType(blockType) {
    this.onChange(
      RichUtils.toggleBlockType(
        this.state.editorState,
        blockType
      )
    );
  }

  _toggleInlineStyle(inlineStyle) {
    this.onChange(
      RichUtils.toggleInlineStyle(
        this.state.editorState,
        inlineStyle
      )
    );
  }

  render() {
    const { editorState } = this.state;

    let className = 'RichEditor-editor';
    var contentState = editorState.getCurrentContent();
    if (!contentState.hasText()) {
      if (contentState.getBlockMap().first().getType() !== 'unstyled') {
        className += ' RichEditor-hidePlaceholder';
      }
    }

    return (
      <div className="RichEditor-root">
        <BlockStyleControls
          editorState={editorState}
          onToggle={this.toggleBlockType}
        />
        <InlineStyleControls
          editorState={editorState}
          onToggle={this.toggleInlineStyle}
        />
        <div className={className} onClick={this.focus}>
          <Editor
            blockStyleFn={getBlockStyle}
            customStyleMap={styleMap}
            editorState={editorState}
            handleKeyCommand={this.handleKeyCommand}
            onChange={this.onChange}
            onTab={this.onTab}
            placeholder="請輸入..."
            ref="editor"
            spellCheck={true}
          />
        </div>
      </div>
    );
  }
}

const styleMap = {
  CODE: {
    backgroundColor: 'rgba(0, 0, 0, 0.05)',
    fontFamily: '"Inconsolata", "Menlo", "Consolas", monospace',
    fontSize: 16,
    padding: 2,
  },
};

function getBlockStyle(block) {
  switch (block.getType()) {
    case 'blockquote': return 'RichEditor-blockquote';
    default: return null;
  }
}

class StyleButton extends React.Component {
  constructor() {
    super();
    this.onToggle = (e) => {
      e.preventDefault();
      this.props.onToggle(this.props.style);
    };
  }

  render() {
    let className = 'RichEditor-styleButton';
    if (this.props.active) {
      className += ' RichEditor-activeButton';
    }

    return (
      <span className={className} onMouseDown={this.onToggle}>
        {this.props.label}
      </span>
    );
  }
}

const BLOCK_TYPES = [
  {label: 'H1', style: 'header-one'},
  {label: 'H2', style: 'header-two'},
  {label: 'H3', style: 'header-three'},
  {label: 'H4', style: 'header-four'},
  {label: 'H5', style: 'header-five'},
  {label: 'H6', style: 'header-six'},
  {label: 'Blockquote', style: 'blockquote'},
  {label: 'UL', style: 'unordered-list-item'},
  {label: 'OL', style: 'ordered-list-item'},
  {label: 'Code Block', style: 'code-block'},
];

const BlockStyleControls = (props) => {
  const {editorState} = props;
  const selection = editorState.getSelection();
  const blockType = editorState
    .getCurrentContent()
    .getBlockForKey(selection.getStartKey())
    .getType();

  return (
    <div className="RichEditor-controls">
      {BLOCK_TYPES.map((type) =>
        <StyleButton
          key={type.label}
          active={type.style === blockType}
          label={type.label}
          onToggle={props.onToggle}
          style={type.style}
        />
      )}
    </div>
  );
};

var INLINE_STYLES = [
  {label: 'Bold', style: 'BOLD'},
  {label: 'Italic', style: 'ITALIC'},
  {label: 'Underline', style: 'UNDERLINE'},
  {label: 'Monospace', style: 'CODE'},
];

const InlineStyleControls = (props) => {
  var currentStyle = props.editorState.getCurrentInlineStyle();
  return (
    <div className="RichEditor-controls">
      {INLINE_STYLES.map(type =>
        <StyleButton
          key={type.label}
          active={currentStyle.has(type.style)}
          label={type.label}
          onToggle={props.onToggle}
          style={type.style}
        />
      )}
    </div>
  );
};

RichEditor.propTypes = {
};

const mapStateToProps = (state) => ({
});

export default connect(mapStateToProps, {
})(radium(RichEditor));
```

2.添加兩份css

Draft.css

```text
/**
 * Draft v0.9.0
 *
 * Copyright (c) 2013-present, Facebook, Inc.
 * All rights reserved.
 *
 * This source code is licensed under the BSD-style license found in the
 * LICENSE file in the root directory of this source tree. An additional grant
 * of patent rights can be found in the PATENTS file in the same directory.
 */
.DraftEditor-editorContainer,.DraftEditor-root,.public-DraftEditor-content{height:inherit;text-align:initial}.public-DraftEditor-content[contenteditable=true]{-webkit-user-modify:read-write-plaintext-only}.DraftEditor-root{position:relative}.DraftEditor-editorContainer{background-color:rgba(255,255,255,0);border-left:.1px solid transparent;position:relative;z-index:1}.public-DraftEditor-block{position:relative}.DraftEditor-alignLeft .public-DraftStyleDefault-block{text-align:left}.DraftEditor-alignLeft .public-DraftEditorPlaceholder-root{left:0;text-align:left}.DraftEditor-alignCenter .public-DraftStyleDefault-block{text-align:center}.DraftEditor-alignCenter .public-DraftEditorPlaceholder-root{margin:0 auto;text-align:center;width:100%}.DraftEditor-alignRight .public-DraftStyleDefault-block{text-align:right}.DraftEditor-alignRight .public-DraftEditorPlaceholder-root{right:0;text-align:right}.public-DraftEditorPlaceholder-root{color:#9197a3;position:absolute;z-index:0}.public-DraftEditorPlaceholder-hasFocus{color:#bdc1c9}.DraftEditorPlaceholder-hidden{display:none}.public-DraftStyleDefault-block{position:relative;white-space:pre-wrap}.public-DraftStyleDefault-ltr{direction:ltr;text-align:left}.public-DraftStyleDefault-rtl{direction:rtl;text-align:right}.public-DraftStyleDefault-listLTR{direction:ltr}.public-DraftStyleDefault-listRTL{direction:rtl}.public-DraftStyleDefault-ol,.public-DraftStyleDefault-ul{margin:16px 0;padding:0}.public-DraftStyleDefault-depth0.public-DraftStyleDefault-listLTR{margin-left:1.5em}.public-DraftStyleDefault-depth0.public-DraftStyleDefault-listRTL{margin-right:1.5em}.public-DraftStyleDefault-depth1.public-DraftStyleDefault-listLTR{margin-left:3em}.public-DraftStyleDefault-depth1.public-DraftStyleDefault-listRTL{margin-right:3em}.public-DraftStyleDefault-depth2.public-DraftStyleDefault-listLTR{margin-left:4.5em}.public-DraftStyleDefault-depth2.public-DraftStyleDefault-listRTL{margin-right:4.5em}.public-DraftStyleDefault-depth3.public-DraftStyleDefault-listLTR{margin-left:6em}.public-DraftStyleDefault-depth3.public-DraftStyleDefault-listRTL{margin-right:6em}.public-DraftStyleDefault-depth4.public-DraftStyleDefault-listLTR{margin-left:7.5em}.public-DraftStyleDefault-depth4.public-DraftStyleDefault-listRTL{margin-right:7.5em}.public-DraftStyleDefault-unorderedListItem{list-style-type:square;position:relative}.public-DraftStyleDefault-unorderedListItem.public-DraftStyleDefault-depth0{list-style-type:disc}.public-DraftStyleDefault-unorderedListItem.public-DraftStyleDefault-depth1{list-style-type:circle}.public-DraftStyleDefault-orderedListItem{list-style-type:none;position:relative}.public-DraftStyleDefault-orderedListItem.public-DraftStyleDefault-listLTR:before{left:-36px;position:absolute;text-align:right;width:30px}.public-DraftStyleDefault-orderedListItem.public-DraftStyleDefault-listRTL:before{position:absolute;right:-36px;text-align:left;width:30px}.public-DraftStyleDefault-orderedListItem:before{content:counter(ol0) ". ";counter-increment:ol0}.public-DraftStyleDefault-orderedListItem.public-DraftStyleDefault-depth1:before{content:counter(ol1) ". ";counter-increment:ol1}.public-DraftStyleDefault-orderedListItem.public-DraftStyleDefault-depth2:before{content:counter(ol2) ". ";counter-increment:ol2}.public-DraftStyleDefault-orderedListItem.public-DraftStyleDefault-depth3:before{content:counter(ol3) ". ";counter-increment:ol3}.public-DraftStyleDefault-orderedListItem.public-DraftStyleDefault-depth4:before{content:counter(ol4) ". ";counter-increment:ol4}.public-DraftStyleDefault-depth0.public-DraftStyleDefault-reset{counter-reset:ol0}.public-DraftStyleDefault-depth1.public-DraftStyleDefault-reset{counter-reset:ol1}.public-DraftStyleDefault-depth2.public-DraftStyleDefault-reset{counter-reset:ol2}.public-DraftStyleDefault-depth3.public-DraftStyleDefault-reset{counter-reset:ol3}.public-DraftStyleDefault-depth4.public-DraftStyleDefault-reset{counter-reset:ol4}
```

RichEditor.css

```text
.RichEditor-root {
  background: #fff;
  border: 1px solid #ddd;
  font-family: 'Georgia', serif;
  font-size: 14px;
  padding: 15px;
}

.RichEditor-editor {
  border-top: 1px solid #ddd;
  cursor: text;
  font-size: 16px;
  margin-top: 10px;
}

.RichEditor-editor .public-DraftEditorPlaceholder-root,
.RichEditor-editor .public-DraftEditor-content {
  margin: 0 -15px -15px;
  padding: 15px;
}

.RichEditor-editor .public-DraftEditor-content {
  min-height: 100px;
}

.RichEditor-hidePlaceholder .public-DraftEditorPlaceholder-root {
  display: none;
}

.RichEditor-editor .RichEditor-blockquote {
  border-left: 5px solid #eee;
  color: #666;
  font-family: 'Hoefler Text', 'Georgia', serif;
  font-style: italic;
  margin: 16px 0;
  padding: 10px 20px;
}

.RichEditor-editor .public-DraftStyleDefault-pre {
  background-color: rgba(0, 0, 0, 0.05);
  font-family: 'Inconsolata', 'Menlo', 'Consolas', monospace;
  font-size: 16px;
  padding: 20px;
}

.RichEditor-controls {
  font-family: 'Helvetica', sans-serif;
  font-size: 14px;
  margin-bottom: 5px;
  user-select: none;
}

.RichEditor-styleButton {
  color: #999;
  cursor: pointer;
  margin-right: 16px;
  padding: 2px 0;
  display: inline-block;
}

.RichEditor-activeButton {
  color: #5890ff;
}
```

之後引入上面包好的Component

即可看到如下畫面

![](螢幕快照%202016-11-09%20下午5.46.00.png)

接著因為我們要將其存成immutable的markdown轉為html，須使用如下模組

[https://github.com/sstur/draft-js-export-html](https://github.com/sstur/draft-js-export-html)

```text
  constructor(props) {
    super(props);
    this.state = { editorState: EditorState.createEmpty() };

    this.onChange = (editorState) => {
      this.setState({ editorState });
      console.log(editorState.getCurrentContent())
      let html = stateToHTML(editorState.getCurrentContent());
      console.log(html)
    }

      //TODO 把此html傳給parent state使用即可
```

如上使用即可

## 使用Material UI

為原本的Materialize css包成的套件。

[http://www.material-ui.com/\#/components/list](http://www.material-ui.com/#/components/list)

ICON: [https://material.io/icons/\#ic\_swap\_horiz](https://material.io/icons/#ic_swap_horiz)

## 使用 I18N

個人推薦：[https://react.i18next.com/guides/quick-start](https://react.i18next.com/guides/quick-start)

他是i18next的react版本，nodejs也有類似，且用法相同，可共用程式。

> 而 [https://github.com/formatjs/react-intl](https://github.com/formatjs/react-intl) 需要且defineMessage與defaultMessage較為繁瑣。

### React hook with lodash Debounce

```javascript
const throttled = useCallback(throttle(newValue => console.log(newValue), 1000), []);
```

e.g.

```javascript
import React, { useState, useCallback } from 'react';
import classnames from 'classnames';
// you should import `lodash` as a whole module
import _ from 'lodash';
import axios from 'axios';

const ITEMS_API_URL = 'https://example.com/api/items';
const DEBOUNCE_DELAY = 500;

// the exported component can be either a function or a class

export default function Autocomplete({ onSelectItem }) {
  const [inputValue, setInputValue] = useState();
  const [listValues, setListValues] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const initListValue = () => {
    setListValues([]);
  }
  const fetchList = async (value) => {
    const { data } = await axios.get(`${ITEMS_API_URL}?q=${value}`);
    return data
  }
  const handleInputChange = async (e) => {
      debounce(e.target.value);
  }
  const debounce = useCallback(_.debounce(async newValue => {
      console.log(newValue)
      try {
        setIsLoading(true);
          initListValue();
          const value = newValue
          if(!value) {
              setIsLoading(false);
              return 
          }
          const data = await fetchList(value);
          setListValues(data);
          setIsLoading(false);
      } catch(err) {
          console.error(err);
      } 
  }, 500), []);
  return (
    <div className="wrapper">
      <div className={classnames("control", isLoading && 'is-loading')}>
        <input onChange={handleInputChange} type="text" className="input" />
      </div>
      {
          listValues.length > 0 && (
            <div className="list is-hoverable">
                {listValues.map(item => (
                    <a onClick={() => onSelectItem(item)} class="list-item" key={item}>{item}</a>
                ))}
            </div>
          )
      }
    </div>
  );
}

```

