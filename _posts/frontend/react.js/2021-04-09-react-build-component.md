---
layout: post
subtitle: "좋은 React Component 설계란"
title: "좋은 Component란?"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - React.js
---

웹애플리케이션의 크기가 커짐에 따라서 웹 컴포넌트의 설계가 중요해졌다. 잘 설계된 프로젝트는 리팩토링이 상대적으로 용이하며, 반복적인 일을 하지 않아도 된다. 그렇다면 어떻게 웹 컴포넌트를 잘 설계할까?

## 1. 공통적인 View, 기능 찾기

웹사이트의 디자인과 기획서를 받은 후 개발을 시작할 때 가장 중요한 포인트라고 생각한다. 우선 컴포넌트의 계층을 잘 나누어 개발할 때 참고해야한다. 이후, 공통적인 기능을 하는 함수들을 제작하여 각 View에 맞게 재사용을 하는 것이다. View는 보통 공통적으로 사용되는 버튼이나 타이틀 등을 기획된 디자인을 통해서 비교적 쉽게 알아볼 수 있다. 하지만 View에 대한 기능은 어떻게 분리를 시켜야 할까?

## 2. 기능 분리 시켜 재사용하기

나는 리엑트를 주로 사용할 때 주로 반복되는 기능이 많았던, text input창에 대한 onChange 함수를 새로 작성하는 것을 예로 들고싶다. onChange는 단순히 이벤트 객체를 받아와 텍스트를 뽑아 상태로 저장시키는 것이 공통된 것이라고 생각하면 되겠다.

```jsx
import React from 'react';

const useTextChange = () => {
	const [value, setValue] = React.useState('');

	const onChange = (e) => {
		setValue(e.target.value);
	}

	return {
		value, onChange
	}
};

export default useTextChange;
```

hooks를 사용자가 새로 제작할 때는, React가 사용자가 작성한 새로훈 hooks인 것을 알 수 있게 React 공식 문서는 사용자 hooks를 use...로 작명하길 권장한다.

## 3. html과 기능의 분리

나는 이전에 스타트업에서 인턴할 당시, Container/Presenter 패턴으로 리엑트 컴포넌트를 작성하였다. 당시에는 '이게 무슨 소용이지?'라는 생각을 하였지만 기능과 View를 간편하고 직관적으로 분리한다는 점에서는 나름 좋은 방법이라는 것을 깨달았다.

Container는 컴포넌트가 반환하는 html 태그에 대한 기능을 담고있는 곳이다. Presenter는 Container에서 제작된 state를 props로 받아와 html태그를 반환한다. 예시는 아래와 같다.

```jsx
const FormContainer = () => {
	const { value: id, onChange: onChangeId } = useTextChange();
	const { value: pw, onChange: onChangePw } = useTextChange();

	return (<FormPresenter 
		id={id}
		pw={pw}
		onChangePw={onChangeId}
		onChangePw={onChangePw}
	/>);
};

const FormPresenter = ({ id, pw, onChangeId, onChangePw }) => (
	<>
		<input name={'id'} type={'text'} value={id} onChange={onChangeId} />
		<input name={'pw'} type={'text'} value={pw} onChange={onChangePw} />
	</>
);
```

만약 html 태그에 문제가 있거나 UI를 수정해야 한다면 Presenter, 단지 기능에 대한 일이라면 Container를 확인하면 된다.

## 4. 꼭 hooks를 써야해? class는?

앞서 설명한 것은 모두 React의 hooks를 사용하였다. 하지만 React의 초창기부터 사용해온 많은 회사들은 class를 기반으로 컴포넌트를 작성해왔다. class내에 hooks를 사용 못하는 제약 때문에 아직도 class를 사용하는 회사들이 몇몇 있다.

이는 잘못된 것이 아니지만, React가 hooks를 밀고있는 만큼, hooks로의 작성이 개인적으로는 필요해 보인다. 그렇다면 class는 왜 지양하며 hooks는 지향할까

### A. 코드가 길고 복잡하다.

constructor, this, binding 등 지켜야 할 규칙이 많아서 코드가 복잡하고 길어진다.클래스 자체가 Life Cycle method로 인해 기본적으로 뚱뚱하다.

### B. 컴포넌트 사이에서 상태와 관련된 Logic을 재사용하기 어렵다.

클래스형 컴포넌트에서는 High-Order Components(HOC)로 컴포넌트 자체를 재사용 할 수는 있지만 부분적인 DOM 관련 처리나 API사용 및 state을 다루는 등의 logic에 있어서는 경우에 따라 같은 로직을 2개 이상의 Life Cycle method에 중복해서 넣어야하는 등 재사용에 제약이 따른다.

→ class도 재사용이 가능하네요?

- class는 제약이 따르는 반면 hooks를 활용한 함수형 컴포넌트에서는 원하는 기능을 함수로 만든 후(hook) 필요한 곳에 훅 집어 넣어주기만 하면 되기 때문에 로직의 재사용이 가능해 집니다.

```jsx
/* React 공식 HoC 예시 */
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);

// 이 함수는 컴포넌트를 매개변수로 받고..
function withSubscription(WrappedComponent, selectData) {
  // ...다른 컴포넌트를 반환하는데...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... 구독을 담당하고...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... 래핑된 컴포넌트를 새로운 데이터로 랜더링 합니다!
      // 컴포넌트에 추가로 props를 내려주는 것에 주목하세요.
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

[React 문서](https://ko.reactjs.org/docs/higher-order-components.html)에서 위와같은 예제를 들며 설명하고 있다. 나는 HoC를 써보지는 않았지만, 무척 복잡해 보인다. 또 컴포넌트를 재사용하여 새 기능을 추가할 때도 새로운 컴포넌트를 덮어 사용하는데, 기능이 추가될 때마다 Wrapper가 추가되어 나중에는 Wrapper hell에 빠진다.

![img](https://user-images.githubusercontent.com/49581472/114151452-15fce880-9958-11eb-950a-47c5a7f09639.png)

→ 리팩토링도 힘들고, 컴포넌트를 작게 만드는 것도 힘들며 테스트하기도 어려움