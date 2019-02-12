---
layout: post
title: 알고리즘
excerpt: "알고리즘 문제를 풀어보자!"
categories: [algorithm]
comments: true
---

## 핸드폰 번호 가리기 (level 1)
주어지는 핸드폰넘버의 뒷자리 4자리를 제외한 나머지 영역을 * 처리 하라.

{% highlight javascript %}
var phone = "01022223333";

// 정규식
function solution(phone_number) {
    return phone_number.replace(/\d(?=\d{4})/g, "*");
}

// 기본형
function solution2(phone){
    var result = "*".repeat(phone.length - 4) + phone.slice(-4);
    return result;
}  

console.log(solution(phone));
console.log(solution2(phone));
{% endhighlight%}

## 김서방 찾기 (level 1)
string형 배열 singer의 element중 twice의 위치 x를 찾아, 트와이스는 x에 있다는 string을 반환하는 함수, solution을 완성하세요. singer에 twice는 오직 한 번만 나타나며 잘못된 값이 입력되는 경우는 없습니다.

{% highlight javascript %}
var singer = ["blackpink","twice","apink"];
    
function solution(singer) {

    var idx = singer.indexOf('twice');
    var answer = '트와이스는 ' + idx + '에 있다';

    // var answer = '';

    // for (let i = 0; i < singer.length ; i++) {
    //   if (singer[i] == 'twice') {
    //     answer = '트와이스는 ' + i + '에 있다';
    //   }
    // }

    return answer;
}
{% endhighlight %}

## 같은 숫자 제거 (level 1)
배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려한다. 배열 arr에서 제거 되고 남은 수들을 return 하는 solution 함수를 완성하라. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 함.

arr = [2,2,4,4,0,1,1,] 이면 [2,4,0,1]을 return 한다.

{% highlight javascript %}
function solution(arr) {
    return arr.filter((val,index) => val != arr[index+1]);
}
{% endhighlight %}

## 2019년 a월 b일은 무슨 요일일까요? (level 1)
두 수 a,b를 입력받아 2019년 a월 b일은 무슨 요일인지 맞추시오.

{% highlight javascript %}
function solution(a, b) {
    var date = new Date(2019, a-1, b);
    return date.toString().slice(0,3).toUpperCase();
}

console.log(solution(2,2));
{% endhighlight %}

## K번째수 (level 1)

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

[1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.

{% highlight javascript %}
function solution(array, commands) {
    return commands.map( v => {
        return array.slice(v[0]-1, v[1]).sort((a,b) => a - b).slice(v[2]-1,v[2])[0];
    });
}

var result = solution([1, 5, 2, 6, 3, 7, 4],[[2, 5, 3], [4, 4, 1], [1, 7, 3]]);

console.log(result); // [5,6,3]
{% endhighlight %}

## 완주하지 못한 선수 (level 1)

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하자.

{% highlight javascript %}
function solution(participant, completion) {
    var p = participant.sort(),
        c = completion.sort();
    
    for (var i = 0; i < p.length ; i++) {
        if (p[i] !== c[i]) {
            return p[i];
        }
    }
}

var result = solution(['leo', 'kiki', 'eden'], ['eden', 'kiki'] );
console.log(result); // leo
{% endhighlight %}