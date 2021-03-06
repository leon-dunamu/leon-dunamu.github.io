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

## ๐๐ปโโ๏ธ Why without CRA?

However, as the development progresses, there is a desire to build an app with your own settings `without create-react-app`, and sometimes I experience difficult with `npm eject` in `create-react-app`.

The package I am introducing from now on is suitable for these people.

- Those who want to graduate from `CRA`! CRA๋ฅผ ์กธ์ํ๊ณ  ์ถ์ ์ฌ๋!
- Those who `bother` to set the basics every time! ๋งค๋ฒ ๊ธฐ๋ณธ ์ค์ ์ ํ๊ธฐ ๊ท์ฐฎ์ ์ฌ๋!
- Those who want to `quickly` apply their own settings! ๋น ๋ฅด๊ฒ ์์ ๋ง์ ์ค์ ์ ์ ์ฉํ๊ณ  ์ถ์ ์ฌ๋!

## ๐ WITHOUT CRA

CRA์์ด ํจํค์ง ๋ค์ด๋ก๋ ๋ฐ๊ธฐ

- [Start react with javascript github repository](https://github.com/leon-dunamu/no-create-react-app)

- [Start react with typescript github repository](https://github.com/leon-dunamu/no-create-react-typescript-app)

## ๐๐ปโโ๏ธ Quickly Start

with js:

```bash
$ git clone https://github.com/leon-dunamu/no-create-react-app.git <YOUR_PROJECT_NAME>
```

with ts:

```bash
$ git clone https://github.com/leon-dunamu/no-create-react-typescript-app.git <YOUR_PROJECT_NAME>
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
โโ[PROJECT_NAME]
โ  โโpublic
โ  โ  โโ index.html
โ  โโsrc
โ  โ  โโ App.css
โ  โ  โโ App.tsx(js)
โ  โ  โโ index.tsx(js)
โ  โโ.gitignore
โ  โโ.prettierrc
โ  โโpackage.json
โ  โโREADME.MD
โ  โโtsconfig.json(ts)
โ  โโjsconfig.json(js)
โ  โโwebpack.config.js
```

## ๐ Contains What?

From now on, I will introduce the apps that I have basically built.

- This app has `webpack` and `babel` configured by default.
- It also includes information on `prettier`.
- There is also a package for `typescript`.
- I set it up to support `HMR(react-hot-loader)`.
- The `built` file was made to exist in the `dist` folder.
- The `absolute path` was also applied.

## ๐คท๐ปโโ๏ธ So what's different from CRA?

Even when I `ejected` the `CRA`, it was difficult to reconfigure the settings.
This package is `raw`, so you can add individual settings for each, and at the same time, it already includes basic settings so you can start developing quickly.

๊ทธ๋์ CRA๋ ๋ค๋ฅธ ์ ์ด ๋ฌด์์ด๋๊ณ ?

`CRA`๋ฅผ `eject` ํ์ ๋๋ ์ค์ ์ ์ฌ๊ตฌ์ฑํ๊ธฐ ์ฝ์ง ์๋ ์ ์ ๊ฒช์๋ค. ์ด ํจํค์ง๋ `raw`ํ๋ฏ๋ก ๊ฐ๊ฐ์ ๊ฐ๋ณ์ ์ธ ์ค์ ๋ค์ ์ถ๊ฐ๋ก ํด์ค ์ ์๋ ๋์์ ๊ธฐ๋ณธ์ ์ธ ์ค์ ๋ค์ ์ด๋ฏธ ํฌํจํ๊ณ  ์์ด ๋น ๋ฅด๊ฒ ๊ฐ๋ฐ์ ์์ํ  ์ ์๊ฒ ํด์ค๋ค.

## Contribute

Due to the `high dependence` on other packages, if there is an error or version mismatch problem during use, please always send `ISSUE` or `PULL REQUEST` to the upper github repository.
