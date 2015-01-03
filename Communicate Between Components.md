# Communicate Between Components

parent에서 child로의 통신에서는 props를 전달해서 할 수 있다.

반대로 child에서 parent로의 통신의 경우를 보자: GroceryList 컴포넌트가 배열에 의해 생선된 아이템 리스트를 갖는다고 하자. 리스트의 한 아이템을 클릭하면 그 아이템의 이름을 표시해 보자.

```javascript
var GroceryList = React.createClass({
  handleClick: function(i) {
    console.log('You clicked: ' + this.props.items[i]);
  },

  render: function() {
    return (
      <div>
        {this.props.items.map(function(item, i) {
          return (
            <div onClick={this.handleClick.bind(this, i)} key={i}>{item}</div>
          );
        }, this)}
      </div>
    );
  }
});

React.render(
  <GroceryList items={['Apple', 'Banana', 'Cranberry']} />, mountNode
);
```

부모-자식 관계가 아닌 두 컴포넌트 사이의 통신에는 이벤트 시스템을 이용할 수 있다. componentDidMount()에서 이벤트를 등록하고, componentWillUnmount()에서 이벤트를 해제하고, 이벤트를 받으면 setState()를 호출한다.

