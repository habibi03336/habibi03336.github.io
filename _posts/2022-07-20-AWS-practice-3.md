---

title: \[AWS 실습3\] Amazon VPC 인프라에 데이터베이스 계층 생성 
date: 2022-7-20
categories: 
    - AWS

tags:
    - 소프트웨어 마에스트로
    - 특강
 
---

__목표:__ ALB를 통해 EC2 인스턴스로 트래픽을 라우팅 해보자. 그리고 EC2 인스턴스가 AWS의 관리형 RDS 서비스인 Aurora DB 인스턴스와 프라이빗 서브넷을 통해 통신하도록 해보자. 

<div style="text-align: center;">
    <img src="/assets/img/aws_practice_3.png" alt="aws_practice_3" width="400"/>
</div>
<br>

ALB를 통해 인바운드 네트워크를 타겟 그룹에 설정된 인스턴스에 트래픽을 분산하여 전달할 수 있었다. 새로고침을 할 때마다 서로 다른 가용영역에 있는 인스턴스로부터 response를 받았다. 

프라이빗 서브넷에 있는 EC2 인스턴스와 DB 인스턴스는 사설 IP로 통신을 할 수 있다.





