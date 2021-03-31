---
layout: post
title: "탐색 알고리즘"
subtitle: "frontend 면접 대비 컴퓨터 기초 학습"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - ComputerScience
---

<br/>

## 선형 탐색 - 리니어 서치

왼쪽부터 순서대로 하나씩 확인해 나감

탐색 시작 -> 결과를 찾을떄까지 루프 반복 -> 결과 찾으면 탐색 종료

단순하고 이해하기 쉽고, 구현하기 쉽지만, 효율은 그다지 좋지않습니다.

빅오표기법으로 보면 배열의 크기에 따라 시간이 늘어나므로, O(n)로 볼 수 있습니다.

```jsx
function findIndexLinear(array, condition) {
  for (let i = 0; i < array.length; i++) {
    if (array[i] === condition) {
      return i;
    }
  }
}
```

<br/><br/>

## 이진 탐색 - 바이너리 서치

데이터가 미리 오름차순이나 내림차순으로 정렬되어 있는 경우에 사용

1. 가운데에 있는 요소를 먼저 탐색합니다.
2. 조건이 가운데 요소보다 정렬순서가 빠른지 느린지를 보고, 탐색범위를 좁힙니다.
3. 탐색범위를 좁혔으면 다시 한번 가운데를 탐색해봅니다.
4. 계속 찾을때까지 반복하여 원하는 결과를 찾으면 탐색 종료.

<br/>

이진 탐색법의 빅오표기는 O(logN)으로 나타낼 수 있습니다.

```jsx
function findIndexBinary(array, condition) {
  let head = 0;
  let tail = array.length - 1;
  // 아래와 같이 배열의 크기가 짝수인 경우에는 3가지 옵션이 있습니다.
  // 1. 반올림 (Round)
  // 2. 올림 (Ceil)
  // 3. 내림 (Floor)
  // 이번에는 내림을 이용하여 구현을 해보겠습니다.
  let centerIndex = Math.floor((head + tail) / 2); // 2.5

  while (array[centerIndex] !== condition) {
    // 찾는 결과가 없을 떄 (infinity loop prevent)
    if (head > tail) return "결과를 찾지 못했습니다.";

    if (array[centerIndex] < condition) {
      head = centerIndex + 1;
      centerIndex = Math.floor((head + tail) / 2);
    } else {
      tail = centerIndex - 1;
      centerIndex = Math.floor((head + tail) / 2);
    }
  }

  return `${centerIndex}번째 요소가 일치`;
}
```

<br/><br/>

## 해시 탐색

해시 탐색법은 데이터의 내용과 저장한 곳의 요소를 미리 연계해 둠으로써 극히 짧은 시간 안에 탐색할 수 있도록 고안된 알고리즘

해시 탐색법의 빅오 표기법은 O(1)로 나타낼 수 있습니다.

다만, 해시 충돌이 일어날 경우 O(n)이 될 수 있습니다.

해시 충돌로 인해 모든 bucket의 value들을 찾아봐야 하는 경우도 있기 때문입니다.

- 해시함수

  어떤 값이 주어진 경우, 그 값을 대표하는 숫자를 계산하는 함수

- 해시값

  해시 함수의 계산으로 산출된 값

<br/>

```jsx
// 먼저 배열을 2개 준비합니다.

let arrayA = [12, 25, 36, 20, 30, 8, 42];

// 앞에서 배운 2개의 검색 알고리즘(선형 탐색, 이진 탐색)은
// 배열의 요소를 데이터 수만큼 준비하면 충분했지만,
// 해시 탐색법은 이와 달리 저장하는 데이터의 1.5~2배를 준비해야 합니다.
let arrayB = new Array(11).fill(0);

// 저장할 요소의 배열을 순회하면서
for (let i = 0; i < arrayA.length; i++) {
  // 저장할 공간(첨자)를 계산
  let temp = arrayA[i] % 11;
  // 저장할 공간에 데이터가 없으면
  if (!arrayB[temp]) {
    // 저장합니다.
    arrayB[temp] = arrayA[i];
    // 저장할 공간에 데이터가 있으면
  } else {
    // 데이터가 없는 공간을 찾을때까지
    while (arrayB[temp]) {
      // temp를 증가, 하지만 배열의 크기가 11이므로,
      // 인덱스가 10을 넘으면(배열의 크기를 넘으면 안되기에) 0으로 변경합니다.
      temp < arrayB.length - 1 ? temp++ : (temp = 0);
    }
    // 데이터가 없는 공간을 찾으면 저장합니다.
    arrayB[temp] = arrayA[i];
  }
}

// [ 42, 12, 0, 25, 36, 0, 0, 0, 30, 20, 8 ]
```

```jsx
// 저장한 데이터와 찾을 요소를 Argument로 넣어줍니다.
function findHashData(arr, target) {
  // 저장할 공간에 데이터가 있어서 +1해준 경우를 제외하고는
  // 첨자 = 저장할데이터 % 저장할 배열의 크기입니다..
  let uniqueValue = target % arr.length;

  // 찾고자 하는 값이 맞을 경우가 나올때까지
  while (arr[uniqueValue] !== target) {
    // 결과가 원하는 값이 아니면, (저장할데이터+1) % 저장할 배열의 크기
    uniqueValue = (uniqueValue + 1) % arr.length;
  }
  return `저장 위치는 ${uniqueValue}번째 요소입니다.`;
}

findHashData(arrayB, 42);
// => '저장 위치는 1번째 요소입니다.'
```
