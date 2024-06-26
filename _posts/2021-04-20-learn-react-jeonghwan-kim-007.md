---
layout: single
title:  만들면서 학습하는 리액트(react):준비편 - 순수JS - 007
categories: 
  - learning react by jeonghwan-kim
tags: 
  - react
---

## 준비편 - 순수JS

### [순수JS 1] 탭3

#### 요구사항

> 각 탭을 클릭하면 탭 아래 내용이 변경된다

#### 구현

- TabView.js 수정

```javascript
...
export default class TabView extends View {
  constructor() {
    ...
    this.bindEvents();
  }
  
  bindEvents() {
    delegate(this.elelment, "click", "li", event => this.handleClick(event));
  }

  handleClick(event) {
    console.log(tag, event);
    const value = event.target.dataset.tab;
    this.emit('@change', { value });
  }
  ...
}
...
```

- Controller.js 수정

```javascript
...
export default class Controller {
  ...
  subscribeViewEvents() {
    ...
    this.tabView.on('@change', event => this.changeTab(event.detail.value));
  }

  changeTab(tab) {
    console.log(tag, "changeTab", tab);
    this.store.selectedTab = tab;
    this.render();
  }
}
```

## 참고
- [https://www.inflearn.com/course/%EB%A7%8C%EB%93%A4%EB%A9%B4%EC%84%9C-%ED%95%99%EC%8A%B5%ED%95%98%EB%8A%94-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard](https://www.inflearn.com/course/%EB%A7%8C%EB%93%A4%EB%A9%B4%EC%84%9C-%ED%95%99%EC%8A%B5%ED%95%98%EB%8A%94-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
- [https://jeonghwan-kim.github.io/series/2021/04/05/lecture-react-ready.html](https://jeonghwan-kim.github.io/series/2021/04/05/lecture-react-ready.html)
