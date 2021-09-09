---
title: "React Compound로 컴포넌트 설계하기 (2)"
subtitle: "React Component 조합 및 Render props 사용하기"
layout: post
author: "Seog"
header-style: text
tags:
  - Frontend
  - React.js
---

이 포스트는 [React Compound로 컴포넌트 설계하기 (1)](https://leon-dunamu.github.io/2021/07/20/react-compound-component/) 이후의 포스트입니당

앞선 포스트에서의 예시는 `Compound Components`의 장점을 모두 보여주기에는 조금 부족하였다. `Compound Components`를 `Custom Hooks`나 다른 패턴과 사용하면 상태를 숨김으로 인해 더 깔끔한 추상화를 제공할 수 있다.

## 📚 기본적인 Compound Components 예시

이전 포스트와는 다른 예시를 들겠다

```jsx
// 조합으로 쪼개기 전
export default function Page() {
  const [option, setOption] = React.useState(defaultOption);

  return (
    <Select
      options={options}
      handleSelectOption={setOption}
      selectedOption={option}
    />
  );
}

// 조합으로 쪼갠 후
export default function Page() {
  const [selected, setSelected] = React.useState(defaultOption);

  return (
    <Select>
      {options.map(optionItem => (
        <Select.Option
          value={optionItem}
          isSelected={optionItem === selected}
          handleSelect={setSelected}
        >
          {optionItem}
        </Select.Option>
      ))}
    </Select.Option>
  );
}
```

위와 같이 유연한 컴포넌트를 만들 수 있다. 하지만 현재는 `<Select />`의 상위 컴포넌트인 `<Page />`에 상태가 존자하는데요. 어떻게 상태를 숨길 수 있을까요?

방법은 아래와 같습니다

```jsx
const Page = () => (
  <Select>
    {options.map(optionItem => (
      <Select.Option
        value={optionItem}
      >
        {optionItem}
      </Select.Option>
    ))}
  </Select.Option>
);

const Select = ({children}) => {
  const [selected, setSelected] = React.useState(defaultOption);

  return (
    <ul>
      {React.Children.map((children, child) => (
        React.cloneElement(child, {
          isSelected: child.props.value === selected,
          handleSelect: () => setSelected(child.props.value),
        })
      ))}
    </ul>
  );
}

const Option = ({chlidren, isSelected, handleSelect}) => (
  <li
    onClick={handleSelect}
    className={`option-item ${isSelected ? 'selected' : ''}`}
  >
    {children}
  </li>
)

Select.Option = Option;

export default Select;
```

제가 `Select - Option`으로 예를 들었는데요, 이는 html 태그 중에 `select - option`과 유사하여서 예시로 들었습니다

```html
<select>
  <option>A</option>
  <option>B</option>
  <option>C</option>
  <option>D</option>
</select>
```

다만 위와같이 진행할 경우 `<Select.Option />`는 무조건 `<Select />`의 바로 아래 자식으로 와야합니다. 이렇게되면 `children`의 구조를 유연하게 변경할 수 없습니다

이는 `Context API`를 사용하여 해결할 수 있다

## 🪓 Context API를 이용한 개선

첫 번째 방법은 `Context API`를 이용하는 것이다. 사용법은 다음과 같다.

```jsx
import React from "react";

const SelectContext = React.createContext();

const Select = ({ children }) => {
  const [selected, setSelected] = React.useState(defaultOption);

  return (
    <SelectContext.Provider value={{ selected, setSelected }}>
      <ul>{children}</ul>
    </SelectContext.Provider>
  );
};

const Option = ({ value, children }) => {
  const context = React.useContext(SelectContext);
  if (context === undefined) {
    throw new Error(
      "<Select.Option> 컴포넌트는 <Select> 컴포넌트 아래에서만 사용될 수 있습니다."
    );
  }

  const { selected, setSelected } = context;

  return (
    <li
      onClick={() => setSelected(value)}
      className={`option-item ${selected === value ? "selected" : ""}`}
    >
      {children}
    </li>
  );
};
```

위와 같이 `Context API`를 사용하여 상태를 사용하면 다음과 같이 `Page`를 작성할 수 있다. 다만 `Context API`의 `value`가 바뀌었을 때 하위 컴포넌트 모두 `re-rendering`이 발생한다. 따라서 `<Select />`와 가까이 사용하는 것이 좋다

```jsx
const Page = () => (
  <Select>
    <SomeComponent />
    <Wrapper>
      {options.map((option) => (
        <Select.Option value={option}>{option}</Select.Option>
      ))}
    </Wrapper>
    <OtherComponent />
  </Select>
);
```

## 🤟 Render Props를 이용한 개선

`Render Props`는

> 1. children으로 함수를 전달하여 사용하거나 <br/>
> 2. props로 컴포넌트를 넘겨 사용할 수 있다

예를 들자면 다음과 같다

```jsx
// 1
<Button>
  {(props) => <>CLICK</>}
</Button>

// 2
const Button = ({render}) => (
  <button>
    {render(props)}
  </button>
)

<Button
  render={(props) => <>CLICK</>}
/>
```

왜 굳이 이렇게 하는 방법이 나왔을까? 이는 다음과 같다.

1. `<Select />` 내부의 상태를 바깥에 사용할 수 있다. 이를 통해 상태를 직접 접근하여 유연함이 증가한다. 하지만 유연하다고 좋은 추상화는 아니기에 상황에 맞게 잘 선택하여 사용해야 한다.

2. `React.cloneElement`나 `Context`를 사용할 때보다는 코드의 양이 줄어든다. 대신 상위 컴포넌트의 코드양은 증가하는데 `Render Props까지 적용하는 것이 2차 제어역전`으로 볼 수 있다.

다만 `Render Props`는 렌더링이 일어날 때마다 새로운 `props`로 인식하므로 최적화를 위해 `React.useCallback`을 사용하거나 `Render Props`도입을 고민해보아야 한다.

## 마치며

제어역전은 컴포넌트에 유연함을 줘 재사용성을 높일 수 있다. 하지만 제어역전은 코드양이 많아지고 한눈에 들어오지 않는다는 단점도 지니고 있다. 추후 변경될 여지가 있는 컴포넌트는 오히려 제어역전을 사용하는 것이 추후에 해를 끼칠 수 있다.

### 참고한 컨텐츠

- [brunch\_블로그](https://brunch.co.kr/@finda/556)
