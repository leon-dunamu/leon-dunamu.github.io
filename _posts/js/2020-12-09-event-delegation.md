---
layout: post
subtitle: "javascript 이벤트 위임"
title: "frontend 면접 대비 이벤트 위임 학습"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - Javascript
---

이벤트 위임이란 자식 엘리먼트의 이벤트를 부모엘리먼트에서 감지할 수 있으니 이벤트를 하나하나 등록하는 것이 아니라 부모에게 이벤트를 위임 하는 방법

보통은 child에서 parent로 propagation됨

하지만 capture로 parent에서 child로 전환 가능

```jsx
var divs = document.querySelectorAll("div");
divs.forEach(function (div) {
  div.addEventListener("click", logEvent, {
    capture: true, // default 값은 false입니다.
  });
});

function logEvent(event) {
  console.log(event.currentTarget.className);
}
```

이벤트 위임 막는 법

```jsx
// 이벤트 버블링 예제
divs.forEach(function (div) {
  div.addEventListener("click", logEvent);
});

function logEvent(event) {
  event.stopPropagation();
  console.log(event.currentTarget.className); // three
}
```

출처 : [https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
