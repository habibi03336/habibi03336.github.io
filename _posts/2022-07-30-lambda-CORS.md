---

title: lambda에서 CORS로 POST 요청이 거절되는 경우 헤더 설정 하는 방법
date: 2022-7-30
categories: 
    - CORS
    - Frontend
    - WEB
    - AWS

tags:
    - 개발
 
---

AWS lambda를 활용하여서 간단하게 동작하는 api 서버를 만들고 있는데 api 서버에 post 요청을 했더니 cors policy error가 났다.
그래서 클라이언트에서 preflight 요청이 올 경우(method가 OPTIONS로 올 경우) 다음과 같이 헤더를 설정해서 response하도록 하였다.
```javascript
const allowCORSresponse = {
    statusCode: 200,
    headers: {
    "Access-Control-Allow-Headers": "Content-Type",
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Methods": "OPTIONS,POST,GET",
    }
};
```
postman으로 확인해본 결과 OPTIONS 요청시 응답에 헤더가 잘 전달 되는 것을 확인했다. 하지만 막상 브라우져에서 preflight를 하면 헤더가 잘 전달되지 않았다.

lambda 함수 설정에 문제의 원인이 있었다. lambda의 구성에서 함수 URL을 보면 오리진에게 노출할 헤더와, 오리진으로부터 허용할 헤더를 설정하는 설정이 있다. 함수 상에서 헤더를 넣어준다고 하더라도 이 설정 값에서 노출할 헤더와 허용할 헤더를 설정해주지 않으면 브라우저에게 전달이 되지 않는 것이었다.

<div style="text-align: center;">
    <img src="/assets/img/lambda-cors.png" alt="lambda-cors" width="600"/>
</div>

그런데 다소 의아한 것이 aws 서버 외부로 나갈 때 설정 값에 따라 헤더가 필터링 된다고 볼 수는 없다는 것이다. 왜냐하면 포스트맨으로 요청을 했을 때는 헤더 값들이 전달이 되었기 때문이다. 브라우저 상 요청과 postman에서의 요청에 따라 서로 다른 값을 응답하는 것을 어떻게 이해해야 할까?

