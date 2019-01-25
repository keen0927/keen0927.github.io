---
layout: post
title: storybook.js
excerpt: "리액트에 storybook.js을 적용하여 개발하는 방법에 대한 설명"
categories: [react, storybook]
comments: true
---

# 스토리북 이란?
컴포넌트 단위의 개발을 지원해주는 도구.<br>
개발자가 뷰를 개발할 때 고립된 환경을 제공하여 관심사, 의존성, 환경으로 부터 분리시켜 개발이 가능.<br>
(실리콘밸리의 회사들이 스탠다드 급으로 사용하고 있다고 함. 마이크로소프트, IBM. Airbnb, Slack 등)<br>

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
업데이트 예정

# 참고
[링크 1](https://hyunseob.github.io/2018/01/08/storybook-beginners-guide/)
[링크 2](https://www.youtube.com/watch?v=KnROzZ5Vszg)