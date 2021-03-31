---
title: "Typescript + WebGL (2)"
subtitle: "WebGL 기초 (1)"
layout: post
author: "Seog"
header-style: text
tags: 
  - Typescript
  - Webpack
  - ThreeJS
---

## Typescript + WebGL 목차

<a href="https://goeslog.github.io/frontend/2021/03/29/typescript-setting.html">1. gts, webpack으로 vanilla Typescript 세팅하기</a><br/>
<span style="background-color:skyblue;">2. WebGL 기초 알아보기 (1) - 다운로드 및 사용</span>

<br/>


# Typescript + WebGL (2) 

### WebGL 기초 알아보기 (1) - 다운로드 및 사용

> npm을 이용하여 설치를 진행한다. <br/>
> CDN을 이용할 수도 있다.

```bash
npm install --save three
```

<br/>

패키지 다운로드 후, 아래와 같이 사용할 수 있다.

```javascript
///////////////////////////////////////////////////////
// Option 1: Import the entire three.js core library.
import * as THREE from 'three';

const scene = new THREE.Scene();


///////////////////////////////////////////////////////
// Option 2: Import just the parts you need.
import { Scene } from 'three';

const scene = new Scene();
```

<br />

하지만 다음과 같이 <span style="background-color:#eaeaea;">CDN</span>을 통해 사용할 수도 있다

```javascript
import * as THREE from 'https://unpkg.com/three@<version>/build/three.module.js';

const scene = new THREE.Scene();
```

<br/>

추후에 <span style="background-color:#eaeaea;">loader</span>와 <span style="background-color:#eaeaea;">control</span>들을 <span style="background-color:#eaeaea;">import</span>해야 하는데, 방법은 다음과 같다.
각자 <span style="background-color:#eaeaea;">three.js</span>를 설치한 방법을 토대로 사용하면 된다.

```javascript
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

const controls = new OrbitControls();

// Find the latest version by visiting https://unpkg.com/three.
import { OrbitControls } from 'https://unpkg.com/three@<version>/examples/jsm/controls/OrbitControls.js';

const controls = new OrbitControls();
```


<span style="background-color: ;">
</span>