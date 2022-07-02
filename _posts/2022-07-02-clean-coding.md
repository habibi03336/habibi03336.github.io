---

title: 클린코딩 특강
date: 2022-7-2
categories: 
    - 코드 관리
tags:
    - 특강
 
---

## 클린코딩이란?


대부분의 사람에게 쉽게 이해되도록 코드를 작성하는 것을 '클린코딩'이라고 한다.

쉽게 읽히는 글을 쓰기가 어려운 것 처럼, 쉽게 읽히는 코드를 쓰는 것도 매우 어려운 일이다.

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.  -Martin Fowler-


## 클린코딩이 필요한 이유


'유지보수'의 관점에서 클린코딩은 중요하다. 소프트웨어의 특징 중 하나는 다른 제품에 비해 유지보수에 들어가는 노력이 매우 크다(전체 비용의 60%-90%).


## 클린코딩과 설계

소프트웨어의 복잡도가 어느 수준을 넘어가면 설계가 선행되어야지 클린코딩을 할 수 있다.


## 클린코딩 TIP 
- __자신의 소스코드를 다시 읽는 시간을 가져라.__
- 이름이 길어지더라도 변수, 함수의 의미를 명확히 표현한다.
- 중복적인 연산을 피한다.
- 함수 / 메서드는 작게 만든다. 
- 3중 중첩 이상은 최대한 피한다.
- 주석으로 코드로는 부족한 내용을 설명한다.
- 클래스 이름은 명사가 더 적절한 경우가 많다.
- 소프트웨어 사용 예시를 제시한다. 


## 클린코딩 Practice
조건:
봄/여름에는 딸기와 수박, 가을/겨울에는 사과와 배가 디저트로 제공되는 식당이 있다.

1일나 5일에는 딸기나 사과, 3일이나 7일에는 수박이나 배가 제공된다.

즉, 봄/여름에 1일이나 5일이면 딸기가 제공되고, 3일이나 7일이면 수박이 제공된다. 가을/겨울의 경우 1일이나 5일이면 사과가 제공되고, 3일이나 7일이면 배가 제공된다.

딸기의 경우 1인 당 5개,  
수박의 경우 10인 당 1개,  
사과의 경우 1인당 1개,  
배의 경우 2인 당 2개가 제공된다.  
초과분이 있으면 올림한다.  

봄은 3~5월  
여름은 6~8월  
가을은 9~11월  
겨울은 12~2월  
이다.

월과 식당이용자 수가 제공된 경우 각 일자별 제공되는 디저트를 출력하라.

```javascript
const month = 3; // 월
const peopleNumber = 70; // 식당이용자 수 

// 월에 따른 계절을 받는다.
const getSeasonByMonth = (month) => {
    if(month >= 3 && month <= 5) return 'spring';
    else if(month >= 6 && month <= 8) return 'summer';
    else if(month >= 9 && month <= 11) return 'fall';
    else return 'winter';
};

// 계절에 따른 디저트 매핑
const dessertBySeason = {
    'spring': ['딸기', '수박'],
    'summer': ['딸기', '수박'],
    'fall': ['사과', '배'],
    'winter': ['사과', '배'],
};

// 디저트에 따른 인원 수 매핑
const dessertNumberByPerson = {
    '딸기' : 5,
    '수박' : 1/10,
    '사과' : 1,
    '배': 1/2,
};


const season = getSeasonByMonth(month); // 월에 따른 계절 받기
const desserts = dessertBySeason[season]; // 계절에 따른 디저트 받기

const dessertByDays = []; // 날짜에 따른 디저트 저장
Array(31).fill(0).forEach((_, idx) => {
    const day = idx + 1;
    let dessertOfDay;
 
    switch(day%10){
        case 1:
        case 5:
            dessertOfDay = desserts[0]; //첫 째 자리가 1,5일 => 첫 번째 디저트
            break;
        case 3:
        case 7:
            dessertOfDay = desserts[1]; //첫 째 자리가 3,7일 => 두 번째 디저트
            break;
        default:
            dessertOfDay = null; //아무것도 아니면 null 반한
            break;
    }
    dessertByDays.push(dessertOfDay);
})

//각 날짜에 디저트에 따른 개수 계산과 문자열 생성
const textByDays = dessertByDays.map((dessert, idx) => {
    const day = idx + 1;

    if (dessert == null) return `${month}월 ${day}일: -`; // 디저트가 없는 날 바로 반환

    const dessertNumber = Math.ceil(peopleNumber * dessertNumberByPerson[dessert]); // 초과분 발생시 올림
    return `${month}월 ${day}일: ${dessert} ${dessertNumber}개`;
});

// 각 문자열 newline으로 합침
const ouputText = outputLines.join('\n');

console.log(ouputText);
```

흠... 이 정도면 괜찮은 건가?

---

출처: 2022 소프트웨어 마에스트로 정철웅 멘토님 클린코딩 특강