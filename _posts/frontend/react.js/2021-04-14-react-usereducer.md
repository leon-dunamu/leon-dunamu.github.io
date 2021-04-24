---
title: "React useReducer"
subtitle: "useReducer 를 사용하여 상태 업데이트 로직 분리하기"
layout: post
author: "Seog"
header-style: text
tags: 
  - Frontend
  - React.js
---

## 👣 useReducer 이해하기

나는 주로 `Hooks`를 사용하여 React 컴포넌트를 작성하는데, `useState`를 이용하여 상태관리를 하면 상태가 중구난방으로 관리될 수 있는 것을 겪었다. 다음은 내가 겪었던 관리하기 까다로웠던 간단한 예시이다.

```javascript
const Example = () => {
    const [isLoading, setLoading] = React.useState(true);
    const [data, setData] = React.useState({});
    const [list, setList] = React.useState([]);
    const [isError, setError] = React.useState(false);

    const fetchData = async () => {
        let result = null;

        if (isLoading) {
            setLoading(true);
        }

        try {
            result = await axios.get(URL);
        } catch (e) {
            console.error(e);
        }
        finally {
            if (result) {
                setData(result.data);
                setList(result.list);
            } else {
                setError(true);
            }
            setLoading(false);
        }
    }

    // ... 이하 생략
}
```

이 때 `useReducer`가 필요하다고 생각됐다. 이 Hook 함수를 사용하면 컴포넌트의 상태 업데이트 로직을 컴포넌트에서 분리시킬 수 있다. 상태 업데이트 로직을 컴포넌트 바깥에 작성 할 수도 있고, 심지어 다른 파일에 작성 후 불러와서 사용 할 수도 있다.

## ☝️ useReducer 사용하기

### reducer란?

`reducer`는 현재 상태와 액션 객체를 파라미터로 받아와서 새로운 상태를 반환해주는 함수이다.
action의 `type`과 다음 상태값을 받아 기존의 상태에 추가시킨다.

```javascript
const reducer = (state, action) => {
  switch (action.type) {
    case 'SAVE_DATA':
      return {
          ...state,
          data : action.data
      }
    case 'SAVE_LIST':
      return {
          ...state,
          list : action.list
      };
    default:
      return state;
  }
}
```

### action이란 ?

`action` 은 업데이트를 위한 정보를 가지고 있다. 주로 `type` 값을 지닌 객체 형태로 사용하지만, 꼭 따라야 할 규칙은 따로 없다.

```javascript
{
    type : 'SAVE_DATA',
    data : {
        // data
    }
}

{
    type : 'SAVE_LIST',
    list : [
        // list
    ]
}
```

### useReducer 사용법

여기서 state 는 우리가 앞으로 컴포넌트에서 사용 할 수 있는 상태를 가르키게 되고, `dispatch` 는 액션을 발생시키는 함수이다. 

이 함수는 다음과 같이 사용합니다: `dispatch({ type: 'SAVE_DATA', data })`

그리고 `useReducer`에 넣는 첫번째 파라미터는 `reducer` 함수이고, 두번째 파라미터는 초기 상태이다.

```javascript
const [state, dispatch] = React.useReducer(reducer, initialState);
```

## 👊 useState vs useReducer

어떨 때 `useReducer` 를 쓰고 어떨 때 `useState` 를 써야 할까? 일단, 여기에 있어서는 정해진 답은 없다. 상황에 따라 불편할때도 있고 편할 때도 있다.

예를 들어서 컴포넌트에서 관리하는 값이 딱 하나고, 그 값이 단순한 숫자, 문자열 또는 boolean 값이라면 확실히 `useState` 로 관리하는게 편할 것이다.

하지만, 만약에 컴포넌트에서 관리하는 값이 `여러개가 되어서 상태의 구조가 복잡`해진다면 `useReducer로` 관리하는 것이 편해질 수도 있다.