---
layout: post
title: 자바스크립트 함수 Part.1
excerpt: "프로그래밍 언어를 배우는 데 있어 함수를 마스터하는 것은 중요하며 자바스크립트 (이하JS) 의 경우 더욱 중요하다.
대부분의 프로그래밍 언어가 객체지향기능을 위한 특수 구문을 사용하지만 JS는 함수를 사용한다."
categories: [javascript]
comments: true
---

프로그래밍 언어를 배우는 데 있어 함수를 마스터하는 것은 중요하며 자바스크립트 (이하JS) 의 경우 더욱 중요하다.
대부분의 프로그래밍 언어가 객체지향기능을 위한 특수 구문을 사용하지만 **JS는 함수를 사용**한다.

함수는 실행 문장 집합을 감싸고 있으며, 함수는 JS모듈화의 근간이다. 코드의 재사용, 정보의 구성, 은닉 등에 사용하고 객체의 행위를 지정하는 부분도 사용한다. 

## 함수 객체
자바스크립트에서 함수는 객체이며 프로토타입 객체로 숨겨진 연결을 갖는 이름/값 쌍들의 집합체이다. 객체 중 객체 리터럴로 생성되는 객체는 **Object.prototype**에 연결되고 함수 객체는 **Function.prototype**에 연결된다(Function은 Object.prototype에 연결).
함수는 두 개의 숨겨진 추가속성이 있는데 **함수의 문맥(context)**과 함수의 행위를 구현하는 **코드(code)**이다.

추가로 모든 함수 객체는 **prototype**속성이 있다. 이것은 함수 자체를 값으로 갖는 **constructor**라는 속성이 있는 객체이다.

함수는 객체여서 다른 값들처럼 사용이 가능하다. 변수, 객체, 배열등에 저장되며 인수로도 사용하고 함수의 반환값으로도 사용한다. 

함수를 다른 객체와 구분 짓는 특징은 **호출**이 가능하다.

## 함수 리터럴
함수 객체는 함수 리터럴로 생성할 수 있다.
{% highlight javascript %}
var sum = function (a, b) {
  return a + b;
};
{% endhighlight %}

함수의 이름이 주어지지 않는 경우 익명함수라고 부른다.

함수 리터럴은 표현식이 나올 수 있는 곳이면 어디든지 위치 가능하다. 함수는 다른 함수 내에서도 정의할 수 있다.

## 함수의 구성
  * function 키워드
  * 함수 이름 `함수를 재귀적으로 호출하거나 디버거나 개발 툴에서 함수를 구분할 때도 사용한다.`
  * 함수 매개변수 `일반적인 변수들을 undefined로 초기화하는 것과는 달리 넘겨진 인수로 초기화`
  * 함수 본문 `함수의 몸체(body) 함수를 호출했을때 실행.`
  * return문 `함수는 항상 값을 반환하며 명시적으로 반환하지 않으면 undefined를 반환한다.`

## 함수 호출
함수를 사용하려면 함수를 호출해야 한다. **선택적으로 괄호 안에 매개변수를 제공**할 수도 있다.

함수를 호출하면 현재 함수의 실행을 잠시 중단후 매개변수와 함께 호출한 함수로 넘긴다. 모든 함수는 매개변수에 더해서 **this**와 **arguments**라는 추가적인 매개변수를 받게 된다. 매개변수의 값은 호출하는 패턴에 의해 결정된다. 각각의 패턴에 따라 this를 다르게 초기화 시킨다.

##### 인수 arguments
함수 호출 시 추가적인 매개변수로 arguments란 배열을 사용할 수 있다. 
이 배열은 함수 호출할 떄 전달된 모든 인수에 접근할 수 있게 해준다. 매개변수 개수보다 더 많이 전달된 인수들 또한 포함된다. 
**arguments매개변수는 매개변수의 개수를 정해놓지 않고 넘어오는 인수의 개수에 맞춰서 동작하는 함수를 만들 수 있게 해준다**.
하지만 설계상의 문제로 arguments는 실제 배열은 아니며 배열같은 **객체**이다. arguments는 length라는 속성이 있지만 원래의 배열이 가지고 있는 메소드들은 없기 때문이다. 

##### 메소드 호출 패턴
함수를 객체의 속성에 저장할때 이 함수를 메소드라고 부른다. 메소드 호출시 **this는 메소드를 포함하고 있는 객체에 바인딩**된다.
{% highlight javascript %}
var testObject = {
  value: 0,
  increment: function (num) {
    this.value += typeof num === 'number' ? num : 1;
  }
}

testObject.increment();
console.log(testObject.value); // 1

testObject.increment(2);
console.log(testObject.value); // 3
{% endhighlight %}

메소드는 자신을 포함하고있는 객체의 속성에 접근하기 위해 **this**를 사용할 수 있다. 객체의 값을 this를 사용해서 일거나 변경할 수 있다. 

예외적으로 메소드 호출에서의 this가 함수 안에서 선언되었을때 this는 **window**가 된다.

<br> 이를 방지하려면 변수값에 this를 담아서 불러오면 자신을 포함하고 있는 객체가 this가 된다.

{% highlight javascript %}
var testObject = {
  testArray: ['가','나','다'],
  print: function() {
      var _this = this.testArray;
      setTimeout(function(){
          console.log(_this); 
      },1000);
  }
};
testObject.print(); // testObject
{% endhighlight %}

추가적으로 위의 print메소드를 화살표 함수로 변경하면 this는 window객체가 된다. 이를 해결하려면 화살표 함수가 아닌 function을 사용해서 함수를 사용해야 한다.

> this와 객체의 바인딩은 호출 시 일어난다.

이렇게 늦은 바인딩은 this를 효율적으로 사용하는 함수를 만드는게 가능하고 자신의 객체 문맥을 this로 얻는 메소드를 **퍼블릭메소드**라고 부른다.

##### 함수 호출 패턴
함수가 객체의 속성이 아닌 경우 함수로서 호출
{% highlight javascript %}
var sum = add(1,2);
{% endhighlight %}
함수를 위의 패턴으로 호출 시 **this는 전역객체에 바인딩**된다. 이런 특성은 언어 설계단계에서의 실수이다. 이 문제를 해결하기 위한 방법은 메소드에서 변수를 정의한 후 그곳에 this를 할당하고 내부 함수는 이 변수를 통해 메소드의 this에 접근하면 된다. 
{% highlight javascript %}
// testObject에 double메소드 추가
testObject.double = function() {
  var _this = this; // 대안

  var help = function() {
    _this.value = add(_this.value, _this.value);
  }
  help();
}

testObject.double();
console.log(testObject)
{% endhighlight %}

##### 생성자 호출 패턴 
자바스크립트는 프로토타입에 의해서 상속이 이루어지는 언어이다. 이것은 객체가 자신의 속성 또한 다른 객체에 상속 가능하다는 뜻 이다. 
함수를 new라는 전치 연산자와 함께 호출하면 호출한 함수의 prototype속성의 값에 연결되는 객체가 생성되고, 이 새로운 객체는 this에 바인딩된다.
{% highlight javascript %}
var PrintText = function(string) { // 생성자 함수 생성
  this.status = string;
};

// printText의 모든 인스턴스에 getStatus라는 퍼블릭 메소드를 줌.
PrintText.prototype.getStatus = function() {
  return this.status;
};

// printText의 인스턴스 생성 
var myPrintText = new PrintText('실행됨');
console.log(myPrintText.getStatus()); // 실행됨
{% endhighlight %}
new라는 전치 연산자와 함께 사용하도록 만든 함수를 생성자(`constructor`)라고 한다. 일반적으로 생성자는 파스칼표기법으로 이름을 지정한다. 대문자 표기법을 사용해 함수가 생성자라고 구분하는것은 매우 중요하다.
**생성자 함수를 사용하는 스타일은 권장 사항이 아니다**

##### apply 호출 패턴
apply메소드는 함수를 호출할 떄 사용할 인자들의 배열을 받아온다. 추가로 this의 값을 선택할 수 있도록 해준다. 

apply 메소드에는 **매개변수 2개가 있다.** 첫 번째는 this에 묶이게 될 값이며, 두 번째는 매개변수들의 배열이다.

{% highlight javascript %}
var add = function (a,b) {
  return a + b;
};

var array = [4,5];
var sum = add.apply(null, array);
console.log(sum); // 9

var statusObject = {
  status: 'Right'
}

var status = myPrintText.getStatus.apply(statusObject);
console.log(status); // Right
{% endhighlight %}

## 매개변수
함수 정의 시 함수가 호출될 때 필요한 매개변수(`parameter`)를 지정할 수 있다. 필요하지 않을 수도 있지만 필요한 경우 이를 전달하지 않으면 **undefined**값이 지정된다.

매개변수와 인수(`argument`)는 다르지만 같은 의미로 사용된다. 매개변수는 함수와 함께 정의되고 인수는 호출될 때 함수에 전달된다.

{% highlight javascript %}
function sum(a,b) { // a,b는 함수와 함께 정의되므로 매개변수이다
  return a + b;
}
sum(3,5); // 3,5는 함수가 호출될때 선언된 인수(인자)이다.
{% endhighlight %}

함수가 필요로하는 인수값보다 많은 갯수의 인수를 전달하는 경우 나머지 인수는 **무시**된다. 또한 각 함수에서는 자동으로 arguments값이 생성되는데 이로인해 매개변수의 수에 유연하게 함수를 작성할 수 있다.

{% highlight javascript %}
function getArgs() {
  return arguments; // arguments값 리턴
}
getArgs();
getArgs(1,2,3,4,5,6,7,8); // [1,2,3,4,5,6,7,8]
{% endhighlight %}

## 반환
함수호출시 첫 번째 문장부터 실행하여 함수가 끝나는 } 를 만나면 종료된다. 함수가 끝나면 프로그램의 제어가 **함수를 호출한 부분으로 반환** 된다.

return은 함수의 끝에 도달하기전에 제어를 반활할 수 있다. return문을 실행하면 함수는 나머지 부분을 실행하지 않고 그 즉시 반환된다.

> 함수는 항상 값을 반환하는데 반환값이 지정되지 않은경우 undefined가 반환된다.

함수를 new라는 전치연산자와 함께 실행하고 반환값이 객체가 아닌경우 반환값은 this가 된다.

<br>

###### 참고서적
> 객체지향 자바스크립트 - 베드 안타니, 스토얀 스테파노프<br>자바스크립트 핵심 가이드 - 더글라스 크락포드