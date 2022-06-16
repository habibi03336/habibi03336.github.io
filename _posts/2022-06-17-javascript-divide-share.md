---

title: 자바스크립트로 나눗셈 몫만 구하는 방법
date: 2022-6-17
categories: 
    - javascript
tags:
    - how to
    
---

__자바스크립트에서 나눗셈의 몫만 구할 때는 다음과 같이 구한다.__

```javascript
const share = Math.floor(x/y);
```

--- 

__Why?__  
(추가적인 내용입니다. 읽고 싶은 분만 읽으세요!) 

javascript로 몫을 구하는 방법을 구글 검색 하면 아래와 같이 하면 된다는 글이 많이 나온다.
```javascript
const share = parseInt(x/y);
```

하지만 이 방식은 문제를 일으키는 경우가 있다. 다음을 보자
```javascript
const x = 1;
const y = 1_000_000_000_000;
const share = parseInt(x/y); // share: 1
```
코드 작성자는 share가 0일 것을 기대하겠지만 실제 값은 1이 들어가 있다.

왜 그럴까? MDN 문서를 보면 parseInt()는 string과 특정 진수를 인자로 받아 정수를 반환한다고 되어있다.
예를 들어
```javascript
const a = '0xF';
const a_parse = parseInt(a, 16); // a_parse: 15
```
이렇게 되면 share에는 15라는 값이 들어있다. a 문자열을 16진수로 해석해서 정수 15를 반환한 것이다. 만약 해당 진법으로 불가능한 문자열을 넣으면 NaN을 반환한다.

[MDN: parseInt()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

만약 문자열이 아닌 값을 인자로 넣게 되면 a 값에 대해 ToString 추상연산을 실행한다. 이때 값이 너무 크거나 작아지게 되면 문제가 발생한다.

```javascript
const a = 1_000_000_000_000;
const a_string = a.toString(); // a_string: '1e-12'
```

값이 너무 크거나 작아지면 a.toString()이 지수 표현을 반환하게된다. 그리고 parseInt()는 문자열 인자에 이해할 수 없는 문자가 오게되면 그 앞까지만 정수로 반환한다. 즉 '1e-12'에서 e를 만나게되면 10진수 기준으로 해석할수 없으니 정수 1을 반환한다. 

이와 같은 내부 과정 때문에 기대와는 다른 값이 나올 수 있다. 
