---

title: [AWS 실습1] AWS API를 사용한 EC2 인스턴스 배포
date: 2022-7-20
categories: 
    - AWS

tags:
    - 소프트웨어 마에스트로
    - 특강
 
---

__목표:__ AWS 관리 콘솔, AWS CLI, AWS CloudFormation 3가지 방식을 통해 Amazon EC2 인스턴스를 만들어 보자.


<div style="text-align: center;">
    <img src="/assets/img/aws_practice_1.png" alt="aws_practice_1" width="400"/>
</div>
<br>


AWS API를 쓰는 방법이 3가지 있다는 사실을 알았다. 
1. AWS 콘솔 (id, pwd 인증)  
:일회용 인스턴스를 빠르게 시작하는 경우 사용 
2. AWS CLI (access key, secret key 인증)  
:스크립트를 통해 인스턴스 생성을 자동화 할 때 사용  
3. AWS SDK (access key, secret key 인증)  
:관련 리소스를 함께 시작하거나 코드형 인프라 개념을 사용하려는 경우에 사용

AWS 콘솔 외의 사용 방법은 익숙하지가 않았다.



