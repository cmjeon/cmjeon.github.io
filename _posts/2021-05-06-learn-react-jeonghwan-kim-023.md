---
layout: single
title:  만들면서 학습하는 리액트(react):사용편 - 023
categories: 
  - learning react by jeonghwan-kim
tags: 
  - react
---

## 사용편

### 폼 제출

#### 요구사항

> 엔터를 입력하면 검색 결과가 보인다

#### 구현

```javascript
// main.js
class App extends React.Component {
  ...

  handleSubmit(event) {
    event.preventDefault();
    console.log('handleSubmit', this.state.searchKeyword);
  }

  render() {
    return (
      <>
      <div className="container">
        <form 
          onSubmit={ event => this.handleSubmit(event)}
        >
          ...
        </form>
      </div>
      </>
    );
  }
}
```

## 참고
- [https://www.inflearn.com/course/%EB%A7%8C%EB%93%A4%EB%A9%B4%EC%84%9C-%ED%95%99%EC%8A%B5%ED%95%98%EB%8A%94-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard](https://www.inflearn.com/course/%EB%A7%8C%EB%93%A4%EB%A9%B4%EC%84%9C-%ED%95%99%EC%8A%B5%ED%95%98%EB%8A%94-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
- [https://jeonghwan-kim.github.io/series/2021/04/12/lecture-react-usage.html](https://jeonghwan-kim.github.io/series/2021/04/12/lecture-react-usage.html)
