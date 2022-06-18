---

title: 자바스크립트 빈 배열 생성하는 방법
date: 2022-6-18
categories: 
    - javascript
tags:
    - how to
    
---

데이터를 처리하다보면 count를 할 때나 중간 데이터를 저장할 때 빈 배열을 활용하는 경우가 많다. 
자바스크립트로 빈 배열을 생성하는 방법은 다음과 같다.

```javascript
const n = 10 // array size
const emptyArray = Array(n).fill()
```


---

Array(n)만으로 충분하지 않은 이유는 Array(n)은 length 프로퍼티만 n으로 설정한 object를 return하기 때문이다. fill()을 하면 length에 따라 0,1,2,3 ... n-1 프로퍼티를 만들고 undefined로 초기화 한다. map과 같은 array 메쏘드가 기대처럼 동작하려면 fill()로 프로퍼티 키를 만들고 초기화 해줘야한다.

```javascript
Object.keys(Array(4)) // []
Object.keys(Array(4).fill()) // [ '0', '1', '2', '3' ]
```

