---

title: \[AWS 실습4\] Amazon VPC에서 고가용성 구성
date: 2022-7-21
categories: 
    - AWS

tags:
    - 소프트웨어 마에스트로
    - 특강
 
---

__목표:__ ELB와 EC2 Auto scailing을 통해 고가용성 웹서버를 구성해본다.

<div style="text-align: center;">
    <img src="/assets/img/aws_practice_4.png" alt="aws_practice_4" width="400"/>
</div>
<br>

ELB는 AWS에서 제공하는 로드 밸런스 서비스이다. Auto scailing과 함께 사용하면 장애 대응을 보다 수월하게 할 수 있다.

Auto scailing은 전략에 따라 자동으로 인스턴스를 유연하게 늘렸다가 줄였다가 하는 서비스이다. Auto scailing을 사용하기 위해서는 생성할 인스턴스의 template을 만들고, 어떤 전략에 따라 인스턴스를 늘리고 줄일지 설정해주면 된다. 그리고 이를 특정 로드 밸런서에 연결해주면 된다.

실습에서 한 인스턴스를 삭제하는 장애 시뮬레이션을 해보았는데, Auto Scailing 이를 감지하여 자동으로 인스턴스를 생성하는 것을 확인 할 수 있었다.

특정 보안 그룹을 거친 네트워크에 대해서만 인바운드를 허락하는 방식으로 네트워크를 제어할 수 있다. 이 실습에서는 Load balance의 보안 그룹을 거친 네트워크만 EC2에 접근할 수 있고, EC2의 보안 그룹을 거친 네트워크만 RDS 인스턴스에 접근할 수 있었다. 