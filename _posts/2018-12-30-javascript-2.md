---
layout: post
title: 자바스크립트 함수 Part.2
excerpt: "es6함수, 재귀적 호출, scope, 클로즈, 콜백, 모듈, 커링, 메모이제이션 등에대한 설명"
categories: [javascript]
comments: true
---

## 재귀호출
재귀 함수는 직접,간접적으로 자신을 호출하는 함수이다. 재귀적 호출은 어떤 문제가 유사한 하위 문제로 나뉘고 각각의 문제를 같은 해결 방법으로 처리 할 수 있을 때 사용할 수 있는 강력한 프로그래밍 기법이다. 

일반적으로 재귀함수는 하위 문제를 처리하기 위해 자신을 호출한다.

하노이의 탑 퍼즐은 재귀적 호출을 위한 전형적인 문제이다. 

<img src="https://t1.daumcdn.net/cfile/tistory/122257314C6933D2DA" />

{% highlight javascript %}
var hanoi = function (disc, src, aux, dst) {
  if (disc > 0) {
    hanoi (disc - 1, src, dst, aux);
    console.log('Move disc ' + disc + ' from ' + src + ' to ' + dst);
    hanoi (disc - 1, aux, src, dst);
  }
};

hanoi (3, 'Src', 'Aux', 'Dst');
{% endhighlight %}

hanoi 함수는 옮겨야 할 원반의 수와 사용할 수 있는 기둥 3개를 넘겨 받는다. hanoi는 자신을 재귀적으로 호출할 때 현재 작업하고 있는 원반의 위에 있는 원반을 처리한다. 이러한 작업을 반복하면 마지막에 원반이 없는 상태에서 함수가 호출된다. 이 때 아무것도 하지 않게 되는데 이렇게 함으로써 재귀적 호출이 무한대로 일어나지 않게 된다.

재귀 함수는 웹 브라우저의 DOM같은 트리 구조를 다루는데 효과적이다. 각각의 재귀적 호출이 트리 구조의 항목 하나에 대해 작동하면 효율적으로 트리 구조를 다룰 수 있다.





###### 참고서적
> 객체지향 자바스크립트 - 베드 안타니, 스토얀 스테파노프<br>자바스크립트 핵심 가이드 - 더글라스 크락포드