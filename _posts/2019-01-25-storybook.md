---
layout: post
title: storybook.js
excerpt: "리액트에 storybook.js을 적용하여 개발하는 방법에 대한 설명"
categories: [react]
comments: true
---

# 스토리북 이란?
컴포넌트 단위의 개발을 지원해주는 도구.<br>
개발자가 뷰를 개발할 때 고립된 환경을 제공하여 관심사, 의존성, 환경으로 부터 분리시켜 개발이 가능.<br>
(실리콘밸리의 회사들이 스탠다드 급으로 사용하고 있다고 함. 마이크로소프트, IBM. Airbnb, Slack 등)<br>
**[데모](https://building.coursera.org/coursera-ui/?selectedKind=Welcome&selectedStory=to%20Storybook&full=0&addons=0&stories=1&panelRight=0&addonPanel=storybook%2Factions%2Factions-panel)**

# 사용하는 이유
로직도 중요하고, 보여지는 화면도 중요하다. 이를 위해 스토리북은<br>
독립된 환경에서 컴포넌트 개발이 가능하며, 디자인과 스타일에 좀 더 신경쓰면서 개발이 가능하다.<br>
앱과 별개로 실행되며, 선형적으로 컴포넌트를 나타나게 해준다.<br>
여기어때 서비스에서는 Admin쪽보다는 모바일웹, PC웹, 앱내 웹뷰쪽에 도입하면 개발 생산력에 도움이 될 것 같다.<br>

# 장점
atomic 설계, flow 설계시 적합<br>
목업 state를 제공하고 핫리로딩을 지원하여 interactive하게 개발이 가능하다.<br>
React, Vue의 큰 장점인 재사용가능 컴포넌트를 작성하게끔 도와준다.<br>

# TDD VS SDD
어떤 방식으로 비교가 되는가?<br>
TDD - 복잡한 것을 잘게 분할해서 생각할 수 있게 만들어줌, 테스트 (로직)케이스를 짜고 코드가 통과를 시키도록 만든다.<br>
SDD - 복잡한 컴포넌트들을 분할하여 모듈화 해줌, Story를 다 정의하고, 그거에 맞추어 UI를 개발함.<br>

# 사용방법
**2019.01.25 기준 `getstorybook` 명령어는 deprecated됨**

### 설치&실행
- 스토리북을 전역 모듈로 설치 함 `npm i -g @storybook/cli`
- 설치할 프로젝트의 루트 디렉토리에서 `sb init`명령어를 실행
- 설치 완료 후 `yarn storybook` 실행
- scss대응 : .storybook/webpack.config.js의 rules에 다음 코드를 추가

{% highlight javascript %}
rules: [
      // add your custom rules.
      {
        test: /\.scss$/,
        loaders: ["style-loader", "css-loader", "sass-loader"],
        include: path.resolve(__dirname, "../")
      }
    ],
{% endhighlight %}

- ./storybook/addons.js 파일 생성 후 다음을 추가하여 로거 패널을 등록.

{% highlight javascript %}
import '@storybook/addon-actions/register';
import '@storybook/addon-links/register';
{% endhighlight %}

### 스토리 작성
- 설치 후 stories 폴더가 생성됨. index.stories.js 열어서 개발할 컴포넌트들을 추가
- 가져올 컴포넌트가 TestButton 컴포넌트라고 했을 때.

{% highlight javascript %}
import React from 'react';
import PropTypes from 'prop-types';
 
import './button.scss';
 
const TestButton = (props) => {
  return (
      <button {...props}>버튼 테스트</button>
  )
}
 
TestButton.propTypes = {
    props: PropTypes.string
}
 
export default TestButton
{% endhighlight %}

{% highlight javascript %}
storiesOf('TestButton', module) // TestButton 이라는 카테고리에
  .add('default', () => <TestButton className="button-test" />) // default 버튼
  .add('disabled', () => <TestButton className="button-test" disabled/>) // disabled 버튼
{% endhighlight %}

### 액션 작성
스토리북에서 사용하는 일종의 로깅용 함수. 각종 이벤트와 함께 데이터를 볼 수 있다.
{% highlight javascript %}
.add('with text', () => <Button onClick={action('clicked')}>Hello Button</Button>)
{% endhighlight %}
해당 이벤트에 action()을 추가하여 필요한 로그값을 작성한다.<br>
action자체는 함수를 반환하므로 이벤트에 바인딩시켜도 로그를 남기지 않는다.<br>
꼭 문자열과 함께 함수를 실행시킨 결과를 이벤트에 바인딩시켜야 한다.

### 데코레이터 작성
스토리북의 데코레이터는 자바스크립트의 데코레이터와 무관하다.
단순히 스토리들을 감싸 래핑 컨테이너로서의 역할을 할 뿐이다.
{% highlight javascript %}
storiesOf('TestButton', module)
  .addDecorator((story) => <div style={{ margin: "50px" }}>{story()}</div>)
  .add('default', () => <TestButton className="button-test" onClick={action('button-click!!')} />)
  .add('disabled', () => <TestButton className="button-test" disabled/>);
{% endhighlight %}
위의 코드의 addDecorator를 선언하여 래핑을 한후 필요한 스타일을 적용하면 add되어있는 스토리들은 래핑된 컴포넌트의 스타일안에서 렌더링된다.

### 배경 적용
컴포넌트의 스타일이 희미해서 배경과 겹쳐 정확한 스타일 확인이 힘들 경우 커스텀 배경색을 추가하여 확인이 가능하다.
- 애드온 설치 `yarn add @storybook/addon-backgrounds`
- addons.js에 `import '@storybook/addon-backgrounds/register';`  추가
- config.js에 다음 추가
{% highlight javascript %}
import { addDecorator } from '@storybook/react';
import { withBackgrounds } from '@storybook/addon-backgrounds';

addDecorator(
  withBackgrounds([
    { name: 'white', value: '#fff', default: true },
    { name: 'black', value: '#000' },
  ])
);
{% endhighlight %}
패널을 확인해보면 백그라운드가 추가되어 있으며 해당 백그라운드 클릭시 변경이 가능하다.





# 참고
[링크 1](https://hyunseob.github.io/2018/01/08/storybook-beginners-guide/)<br>
[링크 2](https://www.youtube.com/watch?v=KnROzZ5Vszg)