# Component Specs and Lifecycle

## Component Specifications

```javascript
ReactComponent render()
```

render 메소드는 반드시 요구되는데, 호출되면 this.props와 this.state를 검사하고 하나의 단일한 자식 컴포넌트를 리턴해야 한다. 이 자식 컴포넌트는 단순한 DOM 컴포넌트(<div />, React.DOM.div()) 이거나 혹은 또 다른 정의된 복합 컴포넌트가 될 수 있다.

만약 아무것도 랜더링하고 싶지 않다면 false나 null을 리턴할 수 있다. 그러면 React는 Diff를 하기 위해 내부적으로 <noscript> 태그로 랜더링 할 것이다. 또한 this.getDOMNode()는 null을 리턴할 것이다.

render()함수는 순수함수여야 하는데, 이것은 이 함수가 컴포넌트의 상태를 바꿀 수 없고, 매 호출시마다 같은 결과를 리턴하며, DOM에 쓰거나 읽을 수 없고, 브라우져와 상호작용(예를 들어 setTimeout)할 수 없다는 것을 의미한다. 브라우져와 상호작용이 필요하다면 componentDidMount나 다른 생명주기 함수들에서 해야 한다. render() 함수를 순수하게 만들면 서버 랜더링을 더 실용적으로 만들 수 있으며, 컴포넌트를 생각하는 것이 더 쉬어질 수 있다.


## getInitialState

```javascript
object getInitialState()
```

컴포넌트가 마운트되기 전에 한 번만 호출된다. 리턴값은 this.state으로 사용된다.

## getDefaultProps

```javascript
object getDefaultProps()
```

클래스(인스턴스가 아니다!)가 만들어질 때 한 번 호출되고 캐쉬된다. 매핑상의 값들은 만약 그 prop이 부모의 컴포넌트에 의해 지정된 것이 아니면 this.props에 설정된다. 즉 부모에서 설정된 것이 우선이다. props는 다음처럼 부모에 의해 설정될 수 있다.

```javascript
React.render(
  <Avatar username="pwh" />,
  document.getElementById('example')
);
```

여기서 username이 속성이고 pwh가 값이다.

getDefaultProps()를 사용하면 다음과 같다.

```javascript
getDefaultProps: function () {
  return {username: "pwh"};
}
```

getDefaultProps는 인스턴스가 만들어질 때 호출되기 때문에 this.props에 의존할 수 없다. 게다가 주의할 점은 이 함수에서 리턴된 복합 오브젝트는 모든 인스턴스에 의해 공유된다(카피되지 않고)는 사실이다.

__참고__

- react properties
- Introduction to properties

## propTypes

```javascript
object propTypes
```

컴포넌트에 전달된 prop 검증한다.

## mixins

```javascript
array mixins
```

여러 컴포넌트 사이에서 behavior를 공유할 수 있다.

## statics

```javascript
object statics
```

...게속
