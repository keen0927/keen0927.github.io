---
layout: post
title: 알고리즘
excerpt: "하루에 한개 알고리즘 문제를 풀어보자."
categories: [algorithm]
comments: true
---

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