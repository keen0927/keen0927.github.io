---
layout: post
title: React 성능향상 관련 (shouldComponentUpdate)
excerpt: "shouldComponentUpdate를 이용한 리액트 성능 향상에 대한 포스트."
categories: [React]
comments: true
---

## React의 컴포넌트 리렌더링
React에서는 state값이 변경되거나(setState) 부모 컴포넌트로부터 prop를 전달받으면 해당 컴포넌트 및 자식 컴포넌트들이 리렌더링이 된다. 작은 어플리케이션에서는 문제가 되지 않을 수 있으나 서비스가 커질수록 이런 부분이 중첩된다면 성능에 큰 영향을 미친다.

## 리렌더링 성능 확인 방법
크롬 브라우저의 개발자툴 탭의 "react"탭으로 이동후 "elements"탭 하단에 "톱니바퀴" 버튼을 누르면 작은 팝업창(Preferences)이 하나 뜬다. Highlight Updates를 체크 한후 테스트 해보고자 하는 곳에 클릭 이벤트 라던지 값을 변경하면 리렌더링이 되는부분들에 보더가 노출된다. (빨간색일수록 성능이 안좋음)

<img width="320" src="{{site.url}}/img/2019-05-16-01.jpg" />

## 1. shouldComponentUpdate 를 사용하여 성능 개선
```js
    shouldComponentUpdate(nextProps, nextState) {
        // 이전의 값과 다음의 값을 비교하여 업데이트를 진행할지 설정 (예 : 코드에서는 number값을 비교)
        if (this.state.number !== nextState.number) { 
            return true;
        }
        return false;
    }
```
위 처럼 적용을 하게되면 값을 비교후 렌더링을 진행할지 말지 컨트롤 할 수가 있다.

## 2. PureComponent를 사용
Component와 비슷하지만 기본적으로 1뎁스를 비교(swallow equality check)하도록 shouldComponentUpdate가 적용되어 있는 메소드이다. 따라서 비교해야하는 값이 간단하게 있는 컴포넌트라면 PureComponent를 사용하면 성능향상에 도움을 줄 수 있다. 물론 PureComponent안에서도 shouldComponentUpdate사용이 가능하다.

```JS
import React, { PureComponent } from 'react'
import ChildrenComponent from './components';

export class App extends PureComponent {

  state = {
    number: 0
  }
  
  handlerIncrease = () => {
    this.setState({
      number: this.state.number + 0 // 변경이 없으므로 클릭 이벤
    })
  }

  render() {
    return (
      <div className="App">
        <button onClick={this.handlerIncrease}>리렌더링 테스트 버튼</button>
        <ChildrenComponent />        
      </div>
    )
  }
}

export default App;
```
위의 코드는 **PureComponen**t를 사용한 예제이다. state안의 number값이 변경되면 리렌더링이 이루어지고 변경되지 않으면 리렌더링이 이루어지지 않는다.
반대로 **Component**를 사용했을 시 shouldComponentUpdate메소드를 사용하여 작업을 하지 않았다면 클릭시에 값이 변경되지 않았어도 리렌더링이 발생한다.

정리하자면 비교해야될 값이 많거나 깊을경우 Component를 사용하여 shouldComponentUpdate메소드 작업이 필요하고. 그외에 PureComponent를 사용한다면 성능 향상에 도움이 될 것이다.




