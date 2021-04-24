---
layout: post
subtitle: "web worker 사용하여 일시키기"
title: "worker 일시켜보기"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - Javascript
---

페이지를 제작하다 보면 무거운 데이터 처리를 해야하는 일이 종종 생겼다. 그냥 많은 양의 데이터를 받아서 적용시키다보니 화면이 버벅거리는 현상이 생겼다. 

> 과도하게 await을 하는 경우나 .. <br/> 엄청 큰 데이터 연산을 하는 경우 ..

이는 javascript가 싱글 스레드 방식으로 작동하기 때문이다. 

## 🚶‍♂️ 1. 기본 개념

예를 들어, 웹 데이터를 받아오는데 3초가 걸리고 계산된 좌표를 받아 DOM에 반영한다고 생각해보자. 데이터를 받아오느라 3초간 DOM 업데이트 등의 다른 작업들을 수행할 수가 없다. 사용자 입장에서는 3초간 멍하니 기다릴 뿐이다. 다른 데이터들도 동시에 받아와야하는 경우에는 더 심각해진다. 데이터를 20번 수신하면 3 * 20 = 60초를 기다려야 하는 셈이다.

그래서 생각만하던 `Web Worker`를 적용해 보았다.

`window` 객체 내에 내장된 `Worker`를 사용하여 js 파일을 전달하면 된다.

```javascript
// 같은 폴더 내에 있는 경우
const worker = new Worker('worker.js');

// 다른 폴더에 있는 경우
const worker = new Worker('../static/js/worker.js');
```

worker는 기존의 main과는 다른 스레드를 사용하는 개념이다. worker에서 기존 DOM 객체에 접근하지 못한다. 하지만 message를 송,수신하는 방식을 제공하기에 main과 worker 사이에 message를 통해 작업을 진행할 수 있다.

```javascript
/* main.js */
if(window.Worker) {
    // worker를 지원하는 브라우저인 경우
    // worker를 통해 일을 시킨다
} else {
    // worker를 지원하지 않는 브라우저
}
```

위처럼 Worker를 지원하는 브라우저인 경우에는 Worker를 통해 일을 시켰지만, 지원하지 않는 브라우저인 경우에는 나는 그냥 데이터를 수신하도록 하였다 ..

## 🤸‍♀️ 2. 사용 방법

그렇다면 worker를 생성해서 일을 시켜보자

```javascript
/* main.js */
let worker = null;

if(window.Worker){
    if (worker) {
        // 기존의 worker가 있는 경우 제거 후 사용
        worker.terminate()
    }

    worker = new Worker('worker.js');

    // worker가 전송한 메세지를 받는 함수
    worker.onmessage = (e) => {
        // e.data를 통해 전송된 message를 확인할 수 있다.
        console.log(e.data);

        // ... some code for received data
    }

    // worker가 에러난 경우
    worker.onerror = (e) => {
        console.log('worker error');
    }

    const data = {
        // 작업을 위해 사용될 데이터
        id : 3
    }

    // worker에게 데이터를 전송해줌
    worker.postMessage({
        ...data
    });
}
```

위처럼 `worker`를 생성 후, 에러처리와 사용할 데이터를 전송하는 등의 작업을 진행하면 된다. 그렇다면 `worker`에서는 어떻게 데이터를 사용하고 결과를 전송할까?

worker는 `onmessage`를 통해 main에서 데이터를 받을 수 있다. 이 때 이벤트 객체를 받는데, `e.data`를 이용해서 수신된 데이터에 접근할 수 있다.

결과는 `postMessage` 함수를 통해서 전송할 수 있다.

```javascript
/* worker.js */

// main으로부터 데이터 전송 받음
onmessage = (e) => {
    console.log('Message received from main script');

    const url = `/api/get_list?id=${e.data.id}`
    
    fetch(url, {
        method: 'GET'
    })
        .then(res => res.json())
        .then(response => {
            // 데이터 처리 코드 작성 후
            const body = `${response}`

            // main 스레드에 데이터 전송
            postMessage({
                type: 'success',
                body
            })
        })
        .catch(error =>
            postMessage({
                type: 'error',
                error
            }));
}
```

다만 message 송,수신간에 데이터에 함수를 넣어 사용하는 것은 불가하다. 함수는 복제하거나 전송될 수 없기 때문이라고 한다. 다만 `JSONfn`이라는 플러그인으로 사용할 수 있다하니 자세한 것은 <a href="https://dev.to/localazy/how-to-pass-function-to-web-workers-4ee1">여기</a>를 침고하도록 하자

`Web Worker`는 대부분의 웹에서는 `Worker`를 쓸 정도로 복잡한 작업을 진행하지 않기 때문에 `Web Worker`를 사용할 일이 드물다. 하지만 빅데이터 처리나 웹 게임 등의 경우에는 유용하다. 계산은 워커에게 맡기고 메인 쓰레드는 DOM 업데이트만 담당하면 된다.