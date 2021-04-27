---
title: "React Context API 사용하기"
subtitle: "Context API란 ?!"
layout: post
author: "Seog"
header-style: text
tags: 
  - Frontend
  - React.js
---

## 👀 Context API ?

일반적인 `React`에서 데이터는 위에서 아래로 (즉, 부모로부터 자식에게) props를 통해 전달된다

하지만, 애플리케이션 안의 여러 컴포넌트들에 전해줘야 하는 props의 경우 (로그인 여부, UI 테마) 이 과정이 번거로울 수 있다. 

`context`를 이용하면, 트리 단계마다 명시적으로 props를 넘겨주지 않아도 많은 컴포넌트가 이러한 값을 공유하도록 할 수 있다.

## 💡 언제 써야할까?

React의 상위에서 하위로의 일방적인(?) 상태 전달로 인해 prop drilling이 발생하곤 한다.

### 🪓 prop drilling?

다음은 prop drilling의 간단한 예시이다.

```javascript
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(o => !o)
  return <Switch on={on} onToggle={toggle} />
}

const Switch = ({on, onToggle}) => {
  return (
    <>
      <SwitchMessage on={on} />
      <SwitchButton onToggle={onToggle} />
    </>
  )
}

const SwitchMessage = ({on}) => {
  return <div>The button is {on ? 'on' : 'off'}</div>
}
const SwitchButton = ({onToggle}) => {
  return <button onClick={onToggle}>Toggle</button>
```

예시를 보면 `on`의 상태와 `toggle`이라는 상태 변경 함수가 `Toggle`에서부터 `Switch`를 거쳐 `SwitchMessage`, `SwitchButton`에서 사용된다.

Switch는 props를 사용하지 않고 그저 전달하는 역할만 한다. 지금은 Switch 하나만이 전달해 주는 경우라 조금 간단해 보이지만, 사이에 수 천 개의 컴포넌트가 존재한다면.. 코드를 유지보수하기도 어려워 보인다.

Context는 이러한 prop drilling을 해결하기위해서 탄생하였다. Context의 탄생 배경이 오직 `상태 관리`를 위한 것이 아님을 인지하자. 

## 📚 Context API 사용법

Context API는 러닝 커브가 상당히 낮다고 생각한다. 단순한 개념은 

* 1. Context를 생성하여
* 2. Provider로 제공하고
* 3. 필요한 부분에서 Consumer로 사용한다

이게 끝이다.

### 1. Context 생성하기

```javascript
/* LoginContext.js */
import React from 'react';

export const defaultValue = {
    loggedIn : false,
    setLoggedIn : () => {}
};

export const LoginContext = React.createContext(defaultValue);
```

### 2. Context 제공하기

```javascript
import React from 'react';
import { defaultValue, LoginContext } from './LoginContext';
import Router from './Router';

const App = () => {
    const setLoggedIn = (loggedIn) => {
        setValue({
            ...value,
            loggedIn
        })
    }

    const [value, setValue] = React.useState({
        ...defaultValue, setLoggedIn
    });


    return <LoginContext.Provider value={value}>
        <Router />
    </LoginContext.Provider>
}

export default App;
```

`1.`에서 제작한 Context를 이용하여 Provider로 하위 컴포넌트들을 감싸면된다.

`Provider`는 무조건 최상위 컴포넌트에 작성할 필요는 없으며, 필요한 컴포넌트 겉에만 감싸주면 된다.

이러한 특성으로 인해 Context는 컴포넌트의 트리상에 존재하며, Provider 컴포넌특가 UnMount되면 Context또한 사라진다는 특성이 있다.

이로인해 Context의 개별적인 lifeCycle을 관리하지 않아도 되며, 이러한 부분은 전역 store를 갖는 Redux와 차이를 보인다.

#### Provider 권고 사항

React는 Provider의 value 부분을 내가 적용한 예제와 같이 부모의 상태로 관리하길 권고한다.

새로운 객체를 value로 지정한다면, reference를 비교하는 특성상 Context가 변할 때마다 하위 컴포넌트가 모두 리렌더링 될 수 있기 때문이다.

### 3. Context value 사용하기

```javascript
import React from 'react'
import { LoginContext } from './LoginContext';

export const Main = () => <LoginContext.Consumer>
{/* 이하 내용 */}
<LoginContext.Consumer>;
```

혹은


```javascript
import React from 'react'
import { LoginContext } from './LoginContext';

export const Main = () => {
    const value = React.useContext(LoginContext);

    return <>
        {/* 이하 내용 */}
    </>
}
```

`useContext` hooks를 이용하면 Wrapper Hell에서 어느정도 벗어날 수 있다.

## Context API 단점

### 1. Wrapper Hell

Provider들과 Consumer들로 감싸면 Wrapper Hell이 존재할 수 밖에 없다.

Redux처럼 single context로 만들면 되겠지만 나같으면 이 상황에서는 Context대신 Redux를 사용하겠다.

### 2. 가이드 제공 부족

Context API는 Context 생성, Context 제공, Context 소비 세 가지로 사용하면 된다. 또 reducer, action등을 활용하여 자유롭게 사용 가능하다.

하지만 너무 자유로워, 서로의 코드 리뷰를 해주거나 공유해야하는 협업에서는 문제가 될 수 있다. (개개인의 코딩 스타일 때문에,,)

### 3. 편의 도구 부재

Redux의 강력한 middleware나 time travel debugging 같은 도구를 사용할 수도 없다. 