---
title: "React without Create React App (CRA)"
subtitle: "Use your custom App without CRA! Do not use CRA anymore. Support Typescript also."
layout: post
author: "Seog"
header-style: text
tags: 
  - Frontend
  - React.js
---

Hi. 

I have enjoyed using CRA(create-react-app).

CRA frees you from basic configurations like `Webpack`, `Babel` and so on.

## 💁🏻‍♂️ Why without CRA?

However, as the development progresses, there is a desire to build an app with your own settings `without create-react-app`, and sometimes I experience difficult with `npm eject` in `create-react-app`.

The package I am introducing from now on is suitable for these people.

* Those who want to graduate from `CRA`! CRA를 졸업하고 싶은 사람!
* Those who `bother` to set the basics every time! 매번 기본 설정을 하기 귀찮은 사람!
* Those who want to `quickly` apply their own settings! 빠르게 자신만의 설정을 적용하고 싶은 사람!

## 👉 WITHOUT CRA

CRA없이 패키지 다운로드 받기

* [Start react with javascript github repository](https://github.com/1Seok2/no-create-react-app)

* [Start react with typescript github repository](https://github.com/1Seok2/no-create-react-typescript-app) 

## 🙋🏻‍♂️ Quickly Start

with js:
```bash
$ git clone https://github.com/1Seok2/no-create-react-app.git <YOUR_PROJECT_NAME>
```

with ts:
```bash
$ git clone https://github.com/1Seok2/no-create-react-typescript-app.git <YOUR_PROJECT_NAME>
```

after cloning:
```bash
$ cd <YOUR_PROJECT_NAME>
$ npm install

$ npm run start
```

additionally set git for your repo:
```bash
$ git init
$ git add .
$ git commit -m "create package"
$ git remote set-url origin <YOUR_GIT_REPO>
$ git push -u origin master
```

result:
```text
├─[PROJECT_NAME]
│  ├─public
│  │  └─ index.html
│  ├─src
│  │  ├─ App.css
│  │  ├─ App.tsx(js)
│  │  └─ index.tsx(js)
│  ├─.gitignore
│  ├─.prettierrc
│  ├─package.json
│  ├─README.MD
│  ├─tsconfig.json(ts)
│  ├─jsconfig.json(js)
│  └─webpack.config.js
```

## 📚 Contains What?

From now on, I will introduce the apps that I have basically built.

* This app has `webpack` and `babel` configured by default.
* It also includes information on `prettier`.
* There is also a package for `typescript`.
* I set it up to support `HMR(react-hot-loader)`.
* The `built` file was made to exist in the `dist` folder.
* The `absolute path` was also applied.

## 🤷🏻‍♂️ So what's different from CRA?

Even when I `ejected` the `CRA`, it was difficult to reconfigure the settings. 
This package is `raw`, so you can add individual settings for each, and at the same time, it already includes basic settings so you can start developing quickly.

그래서 CRA랑 다른 점이 무엇이냐고?

`CRA`를 `eject` 했을 때도 설정을 재구성하기 쉽지 않는 점을 겪었다. 이 패키지는 `raw`하므로 각각의 개별적인 설정들을 추가로 해줄 수 있는 동시에 기본적인 설정들은 이미 포함하고 있어 빠르게 개발을 시작할 수 있게 해준다.

## Contribute

Due to the `high dependence` on other packages, if there is an error or version mismatch problem during use, please always send `ISSUE` or `PULL REQUEST` to the upper github repository.