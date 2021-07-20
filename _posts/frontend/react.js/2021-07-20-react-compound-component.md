---
title: "React Compound로 컴포넌트 설계하기 (1)"
subtitle: "React Component 조합 및 제어역전 알아보기"
layout: post
author: "Seog"
header-style: text
tags: 
  - Frontend
  - React.js
---

리액트를 사용하다보면 하위 컴포넌트에 Props를 넘겨줘야하는 일이 생긴다

하지만 기능을 추가하다보면 너무 많은 Props로 고통받기 십상이다

```jsx
<Survey 
  title='어플 설문조사'
  description='불편하신 점은 없으셨나요'
  questions=[{
      label: '앱 편리성',
      answers: [5, 4, 3, 2, 1],
  },{
      label: '앱 호환성',
      answers: [5, 4, 3, 2, 1],
  }]
  ...
/>
```

Props가 이와같이 많아질 경우,
* props마다의 역할이 헷갈리게 된다
* 이를 설명하기 위한 주석/문서화가 필요하다
* 또 다른 기능들을 추가하거나 변경하기 힘들다
* 비슷한 기능의 props들로 비슷한 이름을 가질 확률이 높다

## 💁🏻‍♂️ 컴포넌트의 조합

위와같은 컴포넌트를 조합을 통해 props 문제를 해결할 수 있다

```jsx
// Survey.js
const Survey = ({children}) => <div className='survey-container'>
  {children}
</div>

const Title = ({title}) => <h1>{title}</h1>

const Description = ({description}) => <p>{description}</p>

const Question = ({title, children}) => <div>
  <h2>{title}</h2>
  <ul>{children}</ul>
</div>

const Answer = ({value}) => <li>{value}</li>

Survey.Title = Title;
Survey.Description = Description;
Survey.Question = Question;
Survey.Answer = Answer;

export default Survey;
```

```jsx
// Survey 사용
import Survey from './Survey';

const App = () => {
  return <Survey>
    <Survey.Title title='설문조사' />
    <Survey.Descruotion description='설문조사 해주세요' />
    <Survey.Question title='물음 1.'>
      <Survey.Answer value='답 1' />
      <Survey.Answer value='답 2' />
      <Survey.Answer value='답 3' />
    </Survey.Question>
    <Survey.Title title='설문조사' />
  <Survey>
}
```

이처럼 사용하니 조금 더 직관적으로 되었다

코드양이 더 많고 지저분해 보이긴 하지만 (특히 Prettier를 사용한 후라면 더욱 그렇다) 이전의 props의 불명확성과 컴포넌트의 변경 부분이 명확해졌다 

## 💡 IoC (제어역전, Inversion of Control)

이전의 `<Survey />`를 변환 후와 비교해보면, 역할이 줄어든 것을 알 수 있다

기존의 props를 받아 props를 스스로 배치하고 스타일을 결정하는 역할을 맡았다면, 두 번째의 경우에는 `<Survey />`는 단순히 스타일에 대한 역할만 가지고 있을 뿐이다

이렇게 `특정 역할을 API를 사용하는 쪽으로 넘기는 패턴을 제어역전`이라고 부른다.

이런 제어역전은 우리가 흔히 사용하는 javascript method에 녹아들어있는데, 다음은 그 예시이다.

```javascript
// 일반적
const myDog = findDog(dogs);

// 제어역전
const myDog = dogs.find(dog => dog.name === 'myDog');
```

우선 일반적인 `find`는 나의 강아지를 찾는 것을 명시함으로 선언적인 코드의 장점을 갖지만, 찾는 조건들이 세분화되거나 추가된다면 추가 인자로 옵션을 받아야 하는 등 기존 로직을 변경해야 할 것이다

하지만 두 번째같은 경우에는 무엇을 찾을 건지에 대한 로직을 사용자, 즉 우리 개발자에게 맡기고 있다. 따라서 다른 조건의 경우를 찾거나 변경해야한다면 다른 함수를 넘기면된다. 이로써 기존의 `find`함수의 로직은 건들이지 않고 좀 더 유연하고 깔끔한 코드를 작성할 수 있게 된다

조금 더 추상화하는 것과 `Render Props`는 다음 글에서 진행하겠디 !