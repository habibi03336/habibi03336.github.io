---

title: \[AWS 실습6\] Amazon S3 오리진으로 Amazon CloudFront 배포 구성
date: 2022-7-21
categories: 
    - AWS

tags:
    - 소프트웨어 마에스트로
    - 특강
 
---

__목표:__ S3 버킷을 활용하여 콘텐츠를 저장하고, CDN 서비스인 Cloud Front를 활용하여 데이터 전송을 빠르게 해보자. 또한 S3로의 접근을 Cloud Front를 통해서만 가능하도록 제한하여 보안성을 높여보자.

<div style="text-align: center;">
    <img src="/assets/img/aws_practice_6.png" alt="aws_practice_6" width="400"/>
</div>
<br>

S3 버킷은 오브젝트 스토어이다. 사진, 비디오, 파일 등을 저장하고 이를 URL로 접근할 수 있도록 해준다. S3 bucket policy를 통해 퍼블릭 접근을 제어할 수 있다. 

클라우드 프론트는 CDN 서비스이다. AWS 엣지 로케이션에 에셋을 캐슁하므로써 데이터의 전송 속도를 높일 수 있다. 또한 S3에 대한 직접적인 퍼블릭 접근을 막음으로써 보안적으로도 우수하다. Cloud front에 대한 origin을 설정할 때 OAI를 생성하여 배정할 수 있다. OAI(Origin Access ID)를 통해 사용자가 Cloud front URL로 에셋에 접근할 수 있는지 확인한다. 이렇게 하면 S3 퍼블릭 접근을 완전히 막더라도 Cloud front를 통해서는 접근이 가능하다. 추가적인 조건에 따른 제어는 Cloud Front의 behavior를 통해 할 수 있다.

