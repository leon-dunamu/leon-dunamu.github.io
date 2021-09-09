---
title: "NPM React Scroll Fade Animation"
subtitle: "React Module on Scroll Fade Animation"
layout: post
author: "Seog"
header-style: text
tags:
  - Frontend
  - React.js
---

I published My NPM Module, [react-scroll-fade-animation](https://www.npmjs.com/package/react-scroll-fade-animation)!!

You can use onScroll Animation in React with My module.

## 💁🏻‍♂️ DEMO page

- [DEMO PAGE](https://leon-dunamu.github.io/react-scroll-fade-animation/)

There are simple examples for `react-scroll-fade-animation`.

## 🏭 Properties

### 1. path

Required. Required Type is `string`.

Default Value is 'top'.

Provide Value is

- top
- bottom
- left
- right
- top-bounce
- bottom-bounce
- left-bounce
- right-bounce

### 2. className

> not Required. Required Type is `string`. <br/>
> You can add custom style with `className`.

### 3. style

> not Required. Required Type is `React.CSSProperties`.
> You can add custom style with `inline camel-case style`

### 4. delay

> not Required. Required Type is `number`. <br/>
> Default value is `0`. <br/>
> You can delay scroll animation(`milliseconds`).

### 5. duration

> not Required. Required Type is `number`. <br/>
> Default value is `0`. <br/>
> You can set scroll animation's duration(`milliseconds`).

### 6. offsetHeight

> not Required. Required Type is `number`. <br/>
> Default value is `0`. <br/>
> You can control scroll-item's displayed position. <br/>
> Both negative and positive numbers are possible. <br/>
> If the set value is `negative`, it is displayed earlier. <br/>
> And if it is positive, it is displayed later. <br/>

### 7. reAnimate

> not Required. Required Type is `boolean` <br/>
> Default value is `false` <br/>
> If the setting value is true, the item disappears if it moves back to the top of the view. <br/>
> If you scroll down the screen again, the animation that shows the item works again. Default(`false`) is not showing animation again.

## 💡 EXAMPLE

You can see at [DEMO PAGE](https://leon-dunamu.github.io/react-scroll-fade-animation/)

## Contribute & Github

- [Github](https://github.com/leon-dunamu/react-scroll-fade-animation)

If you have a nice idea or find some bugs, write `ISSUE` of `PULL REQUEST` anything.
