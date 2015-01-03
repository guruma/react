React (가상) DOM 용어
==============

React 용어에는 반드시 구별해야 할 5가지 핵심 타입이 있다.

- [ReactElement / ReactElement Factory](#react-elements)
- [ReactNode](#react-nodes)
- [ReactComponent / ReactComponent Class](#react-components)


React Elements
--------------

React에서 제 1 타입은 ReactElement이다. ReactElement는 4가지 속성(property)을 갖는다: type, props, key, ref. 하지만 메소드는 없고, 그 prototype상에는 아무것도 없다.

React.createElement함수로 ReactElement 오브젝트를 만들 수 있다.

```javascript
var root = React.createElement('div');
```

이 함수의 첫번째 인수는 ReactElement의 type이다. 이것은 일반적인 html tag 이름의 스트링이거나 아니면 ReactComponent Class가 될 수 있다.

DOM에 랜더링하기 위해서는 ReactElement를 만들어서 React.render 함수에 보통의 DOM element(HTMLElement, SVGElement)와 함께 전달한다. ReactElement와 DOM element를 혼동해서는 안된다. ReactElement는 DOM element에 대한 가볍고, 상태가 없는, 가상의 표현체이다. 그것은 가상 DOM (Virtual DOM)이다.

```javascript
React.render(root, document.body);
```

DOM element에서 속성(property)를 추가하려면, React.createElement 함수의 두번째 인수에는 속성 오브젝트를, 세번째 인수에는 자식들(children)를 넘긴다.

```javascript
var child = React.createElement('li', null, 'Text Content');
var root = React.createElement('ul', { className: 'my-list' }, child);
React.render(root, document.body);
```

React JSX를 사용한다면, ReactElement를 다음과 같이 만들 수 있다.

```javascript
var root = <ul className="my-list">
             <li>Text Content</li>
           </ul>;
React.render(root, document.body);
```

__Factories__

ReactElement factory는 특정 type의 속성을 갖는 ReactElement를 만드는 함수이다. React는 factory를 만드는 효율적인 내장 함수를 제공한다.

```javascript
function createFactory(type){
  return React.createElement.bind(null, type);
}
```

이 함수를 사용하면 다음과 같이 ReactElement를 만드는 작업이 간편해진다.

```javascript
var div = React.createFactory('div');
var root = div({ className: 'my-div' });
React.render(root, document.body);
```

div 변수는 div type의 ReactElement를 만드는 factory가 된다. 이 div factory를 사용해서 이후에서는 div ReactElement를 빠르고 간편하게 생성할 수 있다.

React는 일반적인 html tag들에 대한 내장 factory를 제공한다.

```javascript
var root = React.DOM.ul({ className: 'my-list' },
             React.DOM.li(null, 'Text Content')
           );
```

만일 JSX를 사용한다면, factory를 일일이 만들 필요가 없다. JSX가 내부적으로 factory를 알아서 사용하고 있다.


React Nodes
-----------

ReactNode는 다음 중 하나이다.

- `ReactElement`
- `string` (즉 `ReactText`)
- `number` (즉 `ReactText`)
- Array of `ReactNode`s (즉 `ReactFragment`)


ReactNode는 다른 ReactElement의 속성으로 사용되어 자식(children)으로 표현될 수 있다. 이렇게 해서 효과적으로 ReactElement의 tree를 만들 수 있다.


React Components
----------------

ReactElement만을 이용하여 웹페이지를 만들 수 있지만, ReactElement는 stateless인데 반해, ReactComponent를 이용하면 내장 상태를 캡슐화할 수 있다.

ReactComponent Class는 단지 Javascript 클래스(혹은 “생성자 함수”)이다.

```javascript
var MyComponent = React.createClass({
  render: function() {
    ...
  }
});
```

이 생성자(MyComponent)가 호출되면 적어도 render 메소드를 갖는 오브젝트가 리턴될 것이다. 이 오브젝트를 ReactComponent라고 부른다.

```javascript
var component = new MyComponent(props); // never do this
```

테스트를 위해서가 아니라면, 이 생성자 함수를 직접 호출하지 말라. React가 내부적으로 이 생성자 함수를 사용하게 되는 방식이다. ReactComponent 클래스를 createElement함수에 전달해서 ReactElement를 얻는 것이다.

```javascript
var element = React.createElement(MyComponent);
```

혹은 JSX에서 다음과 같이 사용할 수 있다.

```javascript
var element = <MyComponent />;
```

이 element가 React.render 함수에 전해지면, 생성자 함수가 호출되고, ReactComponent가 만들어져서 리턴된다.

```javascript
var component = React.render(element, document.body);
```

React.render 함수에 똑같은 ReactElement와 똑같은 DOM element를 주어 호출하면 항상 같은 component가 리턴된다. 이 component는 상태를 지닌다.

```javascript
var componentA = React.render(<MyComponent />, document.body);
var componentB = React.render(<MyComponent />, document.body);
componentA === componentB; // true
```

만일 직접 ReactComponent를 만든다면 위와 같은 동등성은 보장되지 않을 것이다. 이것이 생성자 함수를 직접 호출하면 안되는 이유이다. 대신에 ReactElement는 구축되기 전의 가상 ReactComponent이다. 요시기 이전과 이후의 ReactElement를 비교하면 새로운 ReactComponent가 만들어졌는지, 아니면 기존의 것이 재사용되었는지를 알 수 있다.

ReactComponent의 render메소드 또 다른 ReactElement를 리턴하는데, 이런 식으로 Component들은 구성가능하게 된다. 최종적으로 ReactElement는 일반적인 DOM element의 인스턴스가 되는 string tag를 지닌 ReactElement로 분해된다.

Formal Type Definitions
-----------------------

__Entry Point__

```
React.render = (ReactElement, HTMLElement | SVGElement) => ReactComponent;
```

__Nodes and Elements__

```
type ReactNode = ReactElement | ReactFragment | ReactText;

type ReactElement = ReactComponentElement | ReactDOMElement;

type ReactDOMElement = {
  type : string,
  props : {
    children : ReactNodeList,
    className : string,
    etc.
  },
  key : string | boolean | number | null,
  ref : string | null
};

type ReactComponentElement<TProps> = {
  type : ReactClass<TProps>,
  props : TProps,
  key : string | boolean | number | null,
  ref : string | null
};

type ReactFragment = Array<ReactNode | ReactEmpty>;

type ReactNodeList = ReactNode | ReactEmpty;

type ReactText = string | number;

type ReactEmpty = null | undefined | boolean;
```

__Classes and Components__

```
type ReactClass<TProps> = (TProps) => ReactComponent<TProps>;

type ReactComponent<TProps> = {
  props : TProps,
  render : () => ReactElement
};
```
