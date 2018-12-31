---
layout: post
title: 자바스크립트 함수 Part.2
excerpt: "재귀호출, scope, 콜백"
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

## 유효범위 (Scope)
프로그래밍 언어의 유효범위는 변수, 매개변수의 접근성과 생존 기간을 제어한다. 유효범위는 이름들이 충돌하는 문제를 덜어주고 메모리를 자동으로 관리해주기 때문에 중요한 개념이다.

ES5에서의 변수(var) 범위는 블록 유효범위를 지원하지 않는다. ES5에서의 변수 사용은 함수에서 사용하는 모든 변수를 함수 첫 부분에서 선언하는것이 좋다.

ES6에서의 변수(const, let)는 블록 유효범위를 갖고있으며 const변수(상수변수)를 사용하는것을 권장한다.

## 스프레드 연산자(ES6)
스프레드 연산자(전개 연산자)는 함수를 호출하거나 배열을 정의할 때 인수를 제공하는데 사용한다. 배열을 받아 배열의 요소를 개별 변수로 분리한다.

{% highlight javascript %}
function sumAll(a,b,c) {
  return a+b+c;
}
var numbers = [4,5,6];
// 배열을 함수의 인수로 전달하는 ES5 방법
console.log(sumAll.apply(null,numbers)); // 15
// ES6 전개연산자
console.log(sumAll(...numbers)) // 15
};
{% endhighlight %}

ES5에서는 배열을 함수의 인수로 전달할 때 apply() 함수를 사용하는데 ES6의 스프레드 연산자는 훨씬 명확한 방법을 제공한다.

배열끼리의 조합을 구성할때 스프레드 연산자를 사용하면 좀 더 간단하게 구현할 수 있다.
{% highlight javascript %}
var firstNumber = ['4','5','6'];
var secondNumber = ['10','11','12'];
var resultNumber = ['1','2','3',...firstNumber,'7','8','9',...secondNumber];
{% endhighlight %}

## 변수 호이스팅
다음은 지역, 전역 범위 지정에 대한 흥미로운 예이다.

{% highlight javascript %}
var a = 123;

function f() {
  alert(a);
  var a = 1;
  alert(a);
}

f();
{% endhighlight %}

결과는 123얼럿을 실행하고 1얼럿을 실행할 것으로 기대할 것이지만 결과는 다르다.

첫 번째 alert은 undefined를 표시한다. **함수 내에서 지역 범위가 전역 범위보다 중요하기 때문이다.** 따라서 지역 변수는 같은 이름의 전역 변수를 덮어 쓴다. 처음 alert은 a변수는 아직 정의 되지 않아서 undefined를 가지고 있는 상태이다. **호이스팅**으로 인해 지역 공간에 존재한다. 

자바스크립트 프로그램이 실행이 새 함수로 들어가면 함수에서 선언된 모든 변수가 호이스팅(함수의 맨위로 이동) 된다. 또한 **선언 만 호이스팅 되고 할당은 원래의 위치에 그대로 남아 있는다.** 위의 예제에서 지역 변수 a의 선언은 맨위로 호이스팅이 되었지만 값이 1인 할당은 되지 않은것이다. 다음과 같은 방식으로 작성된 것과 같다.

{% highlight javascript %}
var a = 123;

function f() {
  var a; // var a = undefined;
  alert(a); // undefined
  a = 1;
  alert(a); // 1
}

f();
{% endhighlight %}

## 콜백함수
{% highlight javascript %}
function multiplyByTwo(a,b,c, callback){
	var i, ar = [];
	for ( i = 0 ; i < 3 ; i++ ) {
		ar[i] = callback(arguments[i] * 2);
  }
	return ar;
}

function addOne(a){
	return a + 1;
}

myarr = multiplyByTwo(1,2,3, addOne); // 3, 5, 7

// 익명 함수를 사용했을 때
multiplyByTwo(1,2,3, function(a){
  return a + 1;
});
{% endhighlight %}



###### 참고서적
> 객체지향 자바스크립트 - 베드 안타니, 스토얀 스테파노프<br>자바스크립트 핵심 가이드 - 더글라스 크락포드