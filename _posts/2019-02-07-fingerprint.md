---
layout: post
title: Fingerprint. 핑거프린트
excerpt: ""
categories: [javascript]
comments: true
---

# 핑거 프린트?
핑거프린트는 브라우저 지문이라고 하여 PC의 그래픽카드의 처리 기술, 브라우저의 랜더링에 따라 이미지, 문자, 선 등의 처리가 다른것을 이용하여 사용자마다 고유값을 가질수 있다고 정의한것이 핑거프린트이다.<br>
관련 논문 [Pixel Perfect: Fingerprinting Canvas in HTML5](https://hovav.net/ucsd/dist/canvas.pdf)<br>
**논문 요약 : 렌더링 되는 이미지는 PC의 GPU에 따라 미묘하게 달라 고유 이미지가 된다.**

# 왜 사용하는가?
웹 광고, 마케팅시 방문자 구별법으로 고전적인 쿠키 방식의 단점을 보완하기 위해 사용. (사용자에 의해서 쿠키차단, 삭제시 추적하기 힘듬)

# 원리
<img src="https://boygeniusreport.files.wordpress.com/2014/07/canvas-fingerprinting-how-it-works.jpg?quality=98&strip=all" alt="" width="400" />

{% highlight javascript %}
var elem = document.createElement('canvas');
var ctx = elem.getContext('2d');
var txt = "finger print text";
ctx.textBaseline = "top";
ctx.font = "14px 'Arial'";
ctx.textBaseline = "alphabetic";
ctx.fillStyle = "#f50";
ctx.fillRect(125,1,62,20);
ctx.fillStyle = "#080";
ctx.fillText(txt, 1, 12);
ctx.fillStyle = "rgba(130, 122, 0, 0.4)";
ctx.fillText(txt, 3, 14);
{% endhighlight %}

JAVASCRIPT CANVAS를 이용하여 이미지를 생성하여 해시값을 뽑아서 서버로 전송하여 사용.

방문한 브라우저들을 위 방법대로 하면 틀린 해시 값이 나온다고 한다.

# 정확성
동일한 컴퓨터 환경에서(OS,CPU,VGA,브라우저) 겹칠 수 있는 브라우저 지문이 생길 수도 있다.<br>
추가로 다른 정보(유저 에이전트, 날짜등)들과 혼합하여 hash값을 생성한다면 겹칠 확률이 낮아진다.

# 플러그인
위의 정보를 토대로 제작된 자바스크립트용 플러그인 **fingerprintjs2**이 있다.
[fingerprintjs2 깃허브](https://github.com/Valve/fingerprintjs2)