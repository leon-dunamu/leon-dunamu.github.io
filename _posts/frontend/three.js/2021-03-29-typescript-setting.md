---
title: "Typescript + WebGL (1) "
subtitle: "gts, webpack으로 vanilla Typescript 세팅하기"
layout: post
author: "Seog"
header-style: text
tags: 
  - Typescript
  - Webpack
  - Frontend
---


## Typescript + WebGL 목차

<span style="background-color:skyblue;">1. gts, webpack으로 vanilla Typescript 세팅하기</span><br/>
<a href="https://1seok2.github.io/2021/03/30/webgl-basic/>2. WebGL 기초 알아보기 (1) - 다운로드 및 사용</a>


# Typescript + WebGL (1) 

### gts, webpack으로 vanilla Typescript 세팅하기

<br/>

### gts로 프로젝트 생성하기

우선 Typescript 프로젝트를 생성한다

```bash
mkdir WebGL     # 프로젝트 폴더 생성
cd WebGL

npx gts init    # gts를 이용하여 typescript 프로젝트 생성

# 생성 이후 vscode로 프로젝트 오픈
code .
```

<br />


<span style="background-color:#eaeaea;">vscode</span>로 오픈한 경우, 다음과 같은 폴더 구조를 띈다.
(필자는 
<span style="background-color:#eaeaea;">.gitignore</span>에 
<span style="background-color:#eaeaea;">node_modules</span>과 
<span style="background-color:#eaeaea;">package-lock.json</span>을 추가하였다)

<img width="200px" src="https://user-images.githubusercontent.com/49581472/112814101-e3323500-90b9-11eb-8fc6-6f0f55d17d4d.png" alt="프로젝트 생성 후 폴더 구조"/>


`tsconfig.json`의 파일을 수정하여 내 설정을 추가하였다.

```json
{
  "extends": "./node_modules/gts/tsconfig-google.json",
  "compilerOptions": {
    "lib": ["es5", "es6", "dom"],
    "rootDir": ".",
    "outDir": "build",  // tsc를 이용할 경우, root/build 폴더에 컴파일된 항목들이 존재한다
    "target": "es6",
    "module": "commonjs",
    "strict": true,
    "removeComments": true,
    "esModuleInterop": true,
    "baseUrl": ".",
    "paths": {  
      "@src/*": [       // 파일 import시 절대경로 사용하기 위함
        "src/*"         // import '../../../assets/style.css' 과 같은 기존의 방법을
      ]                 // import '@src/assets/style.css' 와 같이 사용 가능
    }
  },
  "include": [
    "src/**/*.ts"
  ],
  "exclude": ["node_modules"]
}
```

<br/>

위 세팅을 바탕으로 src를 작성하면, 
<span style="background-color:#eaeaea;">Uncaught ReferenceError: require is not defined at index.ts:1</span> 에러를 겪었었다.


<span style="background-color:#eaeaea;">import ... from</span>이 
<span style="background-color:#eaeaea;">require</span>로 컴파일 되지만, 브라우저에는 
<span style="background-color:#eaeaea;">reqiure</span>이 없어 
<span style="background-color:#eaeaea;">browserify</span>를 사용하라는 솔루션을 찾았다.


<span style="background-color:#eaeaea;">browserify</span>와 
<span style="background-color:#eaeaea;">rollup</span>을 연동하여 사용하려고 했지만, 
<span style="background-color:#eaeaea;">webpack</span>이 좀 더 적절해보여 
<span style="background-color:#eaeaea;">webpack</span>을 사용한다.

<br/>

### 웹팩 설정하기

우선 웹팩 설치를 진행한다.

```bash
npm i -D webpack webpack-cli
npm i -D css-loader file-loader node-sass sass-loader style-loader ts-loader
npm i -D html-webpack-plugin webpack-dev-server
```

필자는 css대신 scss를 사용하기 위해 
<span style="background-color:#eaeaea;">node-sass sass-loader</span>를 추가로 설치하였고, 
<span style="background-color:#eaeaea;">ts-loader</span> 또한 추가로 설치하였다.

html파일 또한 웹팩에 추가하기 위해 
<span style="background-color:#eaeaea;">html-webpack-plugin</span> 플러그인을 추가하였다.

```javascript
// root/webpack.config.js

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.ts',
  },
  module: {                         // 파일 확장자에 대한 loader 설정
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
      {
        test: /\.scss$/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
      {
        test: /\.(png|jpe?g|gif|jp2|webp)$/,
        loader: 'file-loader',
        options: {
          name: 'images/[name].[ext]',
          publicPath: 'public/',
        },
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
    alias: {
      '@src': path.resolve(__dirname, 'src'),
    },
  },
  output: {                         // 웹팩 컴파일 결과물의 위치
    publicPath: '/public/',
    path: path.resolve('./public/'),
    filename: '[name].js',          // entry의 key값인 'main'이 파일이름으로
  },
  devServer: {                      // 서버를 띄우기 위함
    port: 3000,
    hot: true,                      // 저장시 자동으로 새로고침
    contentBase: __dirname + '/public/',
    inline: true,
    watchOptions: {
      poll: true,
    },
  },
  plugins: [
    new HtmlWebpackPlugin({         // html 파일도 컴파일
      template: './src/index.html',
    }),
  ],
};
```


<span style="background-color:#eaeaea;">package.json</span> 또한 설정해준다

```json
{
    ...
    "scripts": {
        "compile": "tsc -w",
        "build": "webpack --watch",
        "start": "webpack serve --open",
        "predeploy": "webpack"
    },
    ...
}
```


<span style="background-color:#eaeaea;">src</span>에 파일들을 추가한다.

```html
<!-- root/src/index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id='app'></div>
</body>
</html>
```

위에서 `<script src="/public/main.js"></script>`를 추가할 필요가 없다.

웹팩에서 알아서 
<span style="background-color:#eaeaea;">head</span> 태그에 
<span style="background-color:#eaeaea;">script</span>태그를 삽입해준다.

<br/>

```typescript
// index.ts

const $app = document.querySelector('#app');

$app &&
  (() => {
    $app.innerHTML = `<div>hi??</div>`;
  })();
```

### 컴파일 & 서버 실행하기

```bash
# webpack으로 컴파일
npm run build

# 서버 오픈
npm run start
```


<span style="background-color:#eaeaea;">localhost:3000</span>에 진입하면 성공!