---
title: "React Hooks 만들어 사용하기"
subtitle: "custom hooks를 사용하여 코드 재사용하기"
layout: post
author: "Seog"
header-style: text
tags: 
  - Frontend
  - React.js
---

## React Hooks?

기존의 React는 class를 바탕으로 작성되어 왔다. 하지만 class 컴포넌트는 여러가지 단점들을 가지고 있었다.

* 컴포넌트 재사용을 하기 어렵다.
* 라이프사이클 함수가 혼란을 일으킨다. 불필요한 반복 또한 존재한다.
* class의 this는 사람과 기계 모두를 혼동시킨다.

이러한 이유로, React에서 Functional 컴포넌트를 사용하기 위해 Hooks가 탄생하였다.

Hooks는 함수형 컴포넌트에서 class 컴포넌트의 상태와 라이프사이클 등의 기능을 사용할 수 있게 해준다.

Hooks의 탄생 배경 및 역할을 어느정도 알았으니 재사용을 하는 방법을 알아보자

## Hooks의 재사용

컴포넌트를 만들다보면, 반복되는 로직이 자주 발생한다. 예를 들어서 input 을 관리하는 코드는 관리 할 때마다 꽤나 비슷한 코드가 반복된다.

이번에는 그러한 상황에 커스텀 Hooks 를 만들어서 반복되는 로직을 쉽게 재사용하는 방법을 알아보쟈.

```javascript
/**
 * input의 onChange에 대한 hooks
 **/
export const useInputChange = () => {
    const [value, setValue] = React.useState('');

    const onChange = (event) => {
        setValue(event.target.value);
    };

    return {
        value, onChange
    };
}
```

위와 같이 input의 onChange에 대한 Hooks를 제작하였으면 다음과 같이 사용할 수 있다.

```javascript
import { useInputChange } from '@src/hooks/useInutChange';

const InputComponent = () => {
    const { value, onChange } = useInputChange();

    return <input 
        type="text" 
        name="text" 
        {...{ value, onChange }}
    />;
}
```

`input` 부분에 props로 `value={value} onChange={onChange}`이나 

```javascript
const input = useInputChange();

return <input {...input} />
```
처럼 작성할 수 있지만 나는 변수를 두번 기입하기 싫어져서 위와 같이 작성하였다.

더 편한 방법이 있다면 누군가 알려줬으면 좋겠네 ..