# React 编程规范

- [基本规则](#基本规则)
- [命名](#命名)
- [声明](#声明)
- [对齐](#对齐)
- [引号](#引号)
- [空格](#空格)
- [属性](#属性)
- [括号](#括号)
- [标签](#标签)
- [方法](#方法)
- [顺序](#顺序)
- [ES6](#ES6)
- [Mixins](#Mixins)

## 基本规则

* 每个文件只包含一个 React 组件
* 使用 `JSX` 语法
* 除非是从一个非 `JSX` 文件中初始化 app，否则不要使用 `React.createElement`

**Class vs React.createClass**

* 除非有更好的理由使用混淆(mixins)，否则就使用组件类继承 `React.Component`。eslint 规则：[react/prefer-es6-class](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md)

```javascript
// bad
const Listing = React.createClass({
  render() {
    return <div />;
  }
});

// good
class Listing extends React.Component {
  render() {
    return <div />;
  }
}
```

## 命名

* **扩展名:** 使用 `js` 作为 React 组件的扩展名
* **文件名:** 文件命名采用帕斯卡命名法，如：`ReservationCard.js`
* **引用名:**  组件引用采用帕斯卡命名法，其实例采用驼峰式命名法。eslint rules: [react/jsx-pascal-case](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

```javascript
// bad
const reservationCard = require('./ReservationCard');

// good
const ReservationCard = require('./ReservationCard');

// bad
const ReservationItem = <ReservationCard />;

// good
const reservationItem = <ReservationCard />;
```
* **组件命名:**  使用文件名作为组件名。例如：`ReservationCard.js` 组件的引用名应该是 `ReservationCard`。然而，对于一个目录的根组件，应该使用 `index.js` 作为文件名，使用目录名作为组件名。

```javascript
// bad
const Footer = require('./Footer/Footer')

// bad
const Footer = require('./Footer/index')

// good
const Footer = require('./Footer')
```

## 声明

* 不要通过 `displayName` 来命名组件，通过引用来命名组件

```javascript
// bad
export default React.createClass({
  displayName: 'ReservationCard',
  // stuff goes here
});

// good
export default class ReservationCard extends React.Component {
}
```

## 对齐

* 对于 `JSX` 语法，遵循下面的对齐风格。eslint rules:  [react/jsx-closing-bracket-location](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

```html
// bad
  <Foo superLongParam="bar"
       anotherSuperLongParam="baz" />

  // good
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  />

  // if props fit in one line then keep it on the same line
  <Foo bar="bar" />

  // children get indented normally
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  >
    <Spazz />
  </Foo>
```

## 引号

* 对于 `JSX` 使用双引号，对其它所有 JS 属性使用单引号

>为什么？因为 JSX 属性[不能包含被转移的引号](http://eslint.org/docs/rules/jsx-quotes)，并且双引号使得如 `"don't"` 一样的连接词很容易被输入。常规的 HTML 属性也应该使用双引号而不是单引号，JSX 属性反映了这个约定。

eslint rules: [jsx-quotes](http://eslint.org/docs/rules/jsx-quotes)

```html
 // bad
  <Foo bar='bar' />

  // good
  <Foo bar="bar" />

  // bad
  <Foo style={{ left: "20px" }} />

  // good
  <Foo style={{ left: '20px' }} />
```

## 空格

* 在自闭和标签之前留一个空格

```html
// bad
<Foo/>

// very bad
<Foo                 />

// bad
<Foo
 />

// good
<Foo />
```

## 属性

* 属性名采用驼峰式命名法

```html
// bad
<Foo
  UserName="hello"
  phone_number={12345678}
/>

// good
<Foo
  userName="hello"
  phoneNumber={12345678}
/>

```

## 括号

* 当组件跨行时，要用括号包裹 JSX 标签。eslint rules: [react/wrap-multilines](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

```javascript
/// bad
  render() {
    return <MyComponent className="long body" foo="bar">
             <MyChild />
           </MyComponent>;
  }

  // good
  render() {
    return (
      <MyComponent className="long body" foo="bar">
        <MyChild />
      </MyComponent>
    );
  }

  // good, when single line
  render() {
    const body = <div>hello</div>;
    return <MyComponent>{body}</MyComponent>;
  }
```

## 标签

* 没有子组件的父组件使用自闭和标签。eslint rules: [react/self-closing-comp](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

```javascript
// bad
  <Foo className="stuff"></Foo>

  // good
  <Foo className="stuff" />
```
* 如果组件有多行属性，闭合标签应写在新的一行上。eslint rules: [react/jsx-closing-bracket-location](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

```javascript
// bad
  <Foo
    bar="bar"
    baz="baz" />

  // good
  <Foo
    bar="bar"
    baz="baz"
  />
```


* 当需要从数组中循环显示一系列组件
When rendering a list of components from an array, do it inline if it makes sense. If the map function is too long or complicated, consider extracting it out into its own method on the component class.

```js
<ul>
    {items.map(function (item, i) {
        return <li key={i}>{item}</li>;
    )}
</ul>;

// or with ES6

<ul>
    {items.map((item, i) => <li key={i}>{item}</li>)}
</ul>;
```



## 方法

* 对 React 组件的自定义方法和事件处理函数使用 `underscore` 前缀

```javascript
// bad
class extends React.Component {
  onClickSubmit() {
    // do stuff
  }

  // other stuff
});

// good
React.createClass({
  _onClickSubmit() {
    // do stuff
  }

  // other stuff
});
```
* 事件处理函数必须以 `_handle{EventName}` 命名，`Props`传递事件处理时必须命名为 `on{EventName}`。

```js
_handleButtonClick: function (e) { /* ... */ }
```

```js
<Foo onButtonClick={this._handleButtonClick} />
```

* render返回的顶层元素必须包含一个和组件对应的className，这对于CSS/LESS的样式命名空间分隔是很有益的。

```js
// Foobar.js
import React from 'react';
import 'Foobar.less';

const Foobar = React.createClass({
    render: function () {
        return <div className="foobar">
            Foobar
        </div>;
    }
});

export default Foobar;
```

```less
// Foobar.less
.foobar {
    // Rules defined here will only apply to Foobar elements
}
```

## 顺序

* 继承 React.Component 的类的方法遵循下面的顺序

1. constructor
2. optional static methods
3. getChildContext
4. componentWillMount
5. componentDidMount
6. componentWillReceiveProps
7. shouldComponentUpdate
8. componentWillUpdate
9. componentDidUpdate
10. componentWillUnmount
11. clickHandlers or eventHandlers like onClickSubmit() or onChangeDescription()
12. getter methods for render like getSelectReason() or getFooterContent()
13. Optional render methods like renderNavigation() or renderProfilePicture()
14. render

* render放在最后的好处是你只要把鼠标滚动到页面底部就能看到组件输出的JSX结构
* 怎么定义 propTypes，defaultProps，contextTypes 等等...

```javascript
import React, { PropTypes } from 'react';

const propTypes = {
  id: PropTypes.number.isRequired,
  url: PropTypes.string.isRequired,
  text: PropTypes.string,
};

const defaultProps = {
  text: 'Hello World',
};

class Link extends React.Component {
  static methodsAreOk() {
    return true;
  }

  render() {
    return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
  }
}

Link.propTypes = propTypes;
Link.defaultProps = defaultProps;

export default Link;
```

* 使用 React.createClass 时，方法顺序如下：

1. displayName
2. propTypes
3. contextTypes
4. childContextTypes
5. mixins
6. statics
7. defaultProps
8. getDefaultProps
9. getInitialState
10. getChildContext
11. componentWillMount
12. componentDidMount
13. componentWillReceiveProps
14. shouldComponentUpdate
15. componentWillUpdate
16. componentDidUpdate
17. componentWillUnmount
18. clickHandlers or eventHandlers like onClickSubmit() or onChangeDescription()
19. getter methods for render like getSelectReason() or getFooterContent()
20. Optional render methods like renderNavigation() or renderProfilePicture()
21. render

eslint rules: [react/sort-comp](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

## ES6

在ES6中，使用这种方式定义一个类：

```js
import React, { Component } from 'react';

const propTypes = { /* ... */ };
const defaultProps = { /* ... */ };

export default class FooComponent extends Component {
    componentWillMount() {}
    componentWillUnmount() {}
    _handleButtonClick() {}
    _myCustomMethod() {}
    render() {}
}

FooComponent.propTypes = propTypes;
FooComponent.defaultProps = defaultProps;
```

* `propTypes` 和 `defaultProps` 在文件的顶部定义成常量，这和ES5的写法一样，这样做的好处是开发者很容易在文件的头部就发现组件的关键信息。

* 如果 `propTypes` 包含很多项内容, 你可以直接从 `react` 中导出 `PropTypes`，这样写的代码会简短许多。

```js
// bad
import React from 'react';

const propTypes = {
    foo: React.PropTypes.string,
    bar: React.PropTypes.number,
    foobar: React.PropTypes.object,
    // etc
};

// good
import React, { PropTypes } from 'react';

const propTypes = {
    foo: PropTypes.string,
    bar: PropTypes.number,
    foobar: PropTypes.object,
    // etc
};
```

# JSX

* Align props when you can't fit them all on a single line. The ending ">" of the opening tag should go on its own line. This makes it clearly visible where the opening tag ends and the element's children begin.

    ```js
    // Bad
    <div className="foo-container"
        data-bar="some value">
        { /* ... */ }
    </div>

    // Good
    <div className="foo-container" data-bar="some value">
        { /* ... */ }
    </div>

    // Good
    <div
        className="foo-container"
        data-bar="some value"
    >
        { /* ... */ }
    </div>
    ```

* Open elements that span multiple lines on the same line. This conserves space because it requires one fewer tab and also saves a couple of lines.

    ```js
    // OK...
    var multilineJsx = (
        <div>
            { /* ... */ }
        </div>
    );

    // Better
    var multilineJsx = <div>
        { /* ... */ }
    </div>;

    // Better
    var multilineJsx = <div
        className="really very long class"
        id="reallyReallyReallyLongId"
        data-foo="some important value"
    >
        { /* ... */ }
    </div>;
    ```

* Variables holding a JSX object can stay on one line if it is short enough.

    ```js
    var singleLineJsx = <h1>My header</h1>;
    ```

* Components without children should be self-closing.

    ```js
    // Bad
    <FooComponent></FooComponent>

    // Good
    <FooComponent />
    ```

* When rendering a list of components from an array, do it inline if it makes sense. If the map function is too long or complicated, consider extracting it out into its own method on the component class.

    ```js
    <ul>
        {items.map(function (item, i) {
            return <li key={i}>{item}</li>;
        )}
    </ul>;

    // or with ES6

    <ul>
        {items.map((item, i) => <li key={i}>{item}</li>)}
    </ul>;
    ```


# jQuery

* In general, avoid using jQuery within React components.
* Never use jQuery in a component for DOM manipulation.
* Performing ajax with jQuery in your component is fine.
* If you're using a jQuery plugin, consider encapsulating the use of the plugin in its own component so that you only have to manage it in a single place.
