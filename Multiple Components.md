# Multiple Components

지금까지 단일 컴포넌트에 데이타를 표시하고 유저 입력을 처리하는 방법에 대해 알아보았다. 다음에는 React의 가장 좋은 특징중의 하나를 살펴보자. 즉 구성가능성.


## 동기: 관심사의 분리(Separation of Concerns)

잘 정의된 인터페이스로 다른 컴포넌트를 이용하는 모듈라 컴포넌트를 만들면, 함수와 클래스를 사용하면서 얻는 똑같은 이점을 누릴 수가 있다. 특히 앱의 관심사를 분리할 수 해서 새로운 컴포넌트를 만들기만 하면 된다. 당신의 어플리케이션을 위한 커스텀 컴포넌트를 만들어서 당신의 도메인에 가장 잘 맞는 UI를 표현할 수 있다.

## 구성의 예

프로파일 사진과 유저 이름을 보여주는 간단한 아바타 컴포넌트를 만들어 보자.

```javascript
var Avatar = React.createClass({
  render: function() {
    return (
      <div>
        <ProfilePic username={this.props.username} />
        <ProfileLink username={this.props.username} />
      </div>
    );
  }
});

var ProfilePic = React.createClass({
  render: function() {
    return (
      <img src={'http://graph.facebook.com/' + this.props.username + '/picture'} />
    );
  }
});

var ProfileLink = React.createClass({
  render: function() {
    return (
      <a href={'http://www.facebook.com/' + this.props.username}>
        {this.props.username}
      </a>
    );
  }
});

React.render(
  <Avatar username="pwh" />,
  document.getElementById('example')
);
```

## 소유권

Avatar 인스턴스는 ProfilePic과 ProfileLink 인스턴스를 소유한다. React에서는 다음 컴포넌트의 props를 설정하는 컴포넌트가 소유자이다. 형식적으로는 컴포넌트 x가 컴포넌트 y의 render()함수에서 만들어진다면, y는 x를 소유한다고 말한다. 앞에서 언급했듯이, 컴포넌트는 자신의 props를 변경할 수 없다. props는 항상 소유자가 설정한 값으로 고정되어 있다. 이렇게 해서 UI의 일관성이 유지된다.

소유자 관계(owner-ownee)와 부모-자식(parent-child)관계를 구분하는 것이 중요하다. 소유자 관계를 React에서 좀 특별한 반면 부모-자식 관계는 일반적인 DOM 관계이다. 위의 예에서 Avatar는 ProfilePic과 ProfileLink를 소유하지만, div는 ProfilePic과 ProfileLink의 부모이다.

## 자식

React 컴포넌트를 만들 때, 추가적인 React Component나 Javascript 표현을 시작 태그와 종료 태그사이에 넣을 수 있다.

```javascript
<Parent><Child /></Parent>
```

자식을 읽기 위해 부모는 this.props.children이라는 특별 prop을 사용할 수 있다.

이보다는 React.Children utilities 를 사용하라.

__자식 일치(Child Reconciliation)__

...계속
