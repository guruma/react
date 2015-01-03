# React DOM 용어
---

React 용어에는 반드시 구별해야 할 5가지 핵심 타입이 있다.

   * ReactElement / ReactElement Factory
   * ReactNode
   * ReactComponent / ReactComponent Class


## React Elements
---
React에서 제 1 타입은 ReactElement이다. ReactElement는 4가지 속성(property)을 갖는다: type, props, key, ref. 하지만 메소드는 없고, 그 prototype상에는 아무것도 없다.

React.createElement함수로 ReactElement 오브젝트를 만들 수 있다.

   var root = React.createElement('div');



