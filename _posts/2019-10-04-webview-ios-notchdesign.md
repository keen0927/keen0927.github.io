---
layout: post
title: CSS로 모바일웹뷰 아이폰 노치디자인 대응하기 (Feat. Safe-area)
excerpt: "아이폰X 부터 적용된 노치디자인을 대응하는법을 알아보자. "
categories: [CSS]
comments: true
---

## 노치디자인?
아이폰X 부터 적용된 노치디자인(일명 탈모 디자인). 사진으로 알아보자.
<p><img src="../img/notch.png" alt="아아 잡스형.." width="100px" /></p>
그러하다..

## 사례
1. 고정 하단 플로팅 버튼이 있을때 홈 버튼과 겹쳐 UX가 불편해진다.
2. 고정 상단 GNB가 있는데 상단 스테이터스 영역까지 뚫어서 사용해야 할 때.

경험상 위 두가지 사례가 가장 많았다. 앱 개발자님께 스테이터스바 영역을 네이티브로 요청할 수 있지만 디자인 통일성을 맞춰야 할 수도 있고 결국 써야한다면 해결방법을 알아보도록 하자.

## 해결
1. 메타태그의 뷰포트 속성에 다음을 추가
```html
viewport-fit=cover
```
2. CSS에 다음값을 추가<br/>
(예시 - 하단에 플로팅된 버튼과 아이폰 홈 버튼의 간격이 겹쳐 간격을 주고 싶을때)
```css
.sample-css  {
  padding: 12px 16px;
  padding: 12px 16px calc(constant(safe-area-inset-bottom) + 12px) 16px;
  padding: 12px 16px calc(env(safe-area-inset-bottom) + 12px) 16px;
} 
```
끝이다..<br/><br/>

코드를 설명하자면 첫라인의 값은 기본값으로 노치디자인이 아닐때 선언되는 부분이다.<br/>
두번째 라인은 기본값과 값은 비슷한데 패딩바텀값에 safe-area값이 계산되어있어 기본 safe-area-inset에 + 12px을 주어 노치디자인에 대응하였다.<br/>
마지막라인은 전 라인과 같지만 env라는 속성만 다르다.<br/>
IOS에서 11.2 업데이트시 constant속성을 없애고 env로 정의하도록 변경이 되었다.<br/>
하지만 업데이트 안한 유저들이 있어 **두번째, 세번째 라인 모두 추가해야 구, 신기종을 전부 대응할 수 있다**.

#### 수정전
<p><img src="https://miro.medium.com/max/868/1*jtSClglMcjqZLlzO6d6AmA.png" width="300px" alt="" /></p>

#### 수정후
<p><img src="https://miro.medium.com/max/876/1*MuA8n_c1iylsAgoBzbqqKQ.png" width="300px" alt="" /></p>

### 추가 팁
SASS를 사용할 경우 함수에 safe-area대응 함수를 만들어서 사용하면 편하다
```scss
// version: env, constant
// direction: top, right, bottom, left
@function safe($version: env, $direction: all, $value: 1px) {

    @if ($direction == all) {
        @if ($value > 0) {
            @return
                calc(#{$value} + #{$version}(safe-area-inset-top))
                calc(#{$value} + #{$version}(safe-area-inset-right))
                calc(#{$value} + #{$version}(safe-area-inset-bottom))
                calc(#{$value} + #{$version}(safe-area-inset-left));
        }
        @else {
            @return
                #{$version}(safe-area-inset-top)
                #{$version}(safe-area-inset-right)
                #{$version}(safe-area-inset-bottom)
                #{$version}(safe-area-inset-left);
        }
    }
    @else {
        @if ($value > 0) {
            @return
                calc(#{$value} + #{$version}(safe-area-inset-#{$direction}));
        }
        @else {
            @return
                #{$version}(safe-area-inset-#{$direction});
        }
    }
}
```

```css
.sampls-css {
    padding-bottom: 20px;
    padding-bottom: safe(constant, bottom, 20px);
    padding-bottom: safe(env, bottom, 20px);
}
```

1. 글을 마무리하며 정확한 페이지는 PC브라우저로 확인하기가 힘들어 웹뷰페이지를 통해 테스트하는게 정확하다.<br>
모바일도 마찬가지. (모바일 safari에서 스크롤시 나오는 주소창과 하단 브라우저 네비게이션 노출로 불편하다)
2. 구버전 대응시 env값을 + 10px 이하로 사용하면 적용이 안되는 현상이 있다. 20px이나 테스트를 통해 간격조정을 할 필요가 있다.
