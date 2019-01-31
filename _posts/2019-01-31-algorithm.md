---
layout: post
title: 알고리즘
excerpt: "하루에 한개 알고리즘 문제를 풀어보자."
categories: [algorithm]
comments: true
---

## K번째수

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