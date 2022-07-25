---

title: \[AWS 실습5\] 서버리스 아키텍처 구축
date: 2022-7-21
categories: 
    - AWS

tags:
    - 소프트웨어 마에스트로
    - 특강
 
---

__목표:__ SNS, AWS lambda, SQS 같은 서버리스 서비스를 활용하여, 이미지를 S3 버킷에 올릴 시 자동으로 프로세싱하는 어플리케이션을 만들어 보자.

<div style="text-align: center;">
    <img src="/assets/img/aws_practice_5.jpg" alt="aws_practice_5" width="400"/>
</div>
<br>

SNS 주제를 만든다. SNS 주제는 이벤트를 전파하는 역할을 한다.

SQS 대기열을 생성하고 SNS 주제를 구독한다. SQS는 이벤트를 큐에 넣어서 하나씩 처리하도록 해준다. 

각 SQS에 대응하는 lambda 함수를 만든다. lambda는 이벤트 핸들러로 처리 코드를 등록하여 사용한다. 

람다의 모니터와 클라우드워치 로그를 활용하여 람다 함수가 성공적으로 작동하는지 확인하고 디버그 할 수 있다.

이 실습을 하는데 람다 함수가 분명히 있는 모듈을 찾을 수 없다는 에러를 계속 냈다..ㅠ