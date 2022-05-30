---

title: 오픈소스 SW특강
date: 2022-5-7
categories:
    - etc
tags:
    - 특강
    - 소프트웨어 마에스트로
---

## 오픈소스 개발에 입문하는 방법

`오픈소스 소프트웨어`는 소스코드가 공개되어있는 소프트웨어이다. 인터넷 저장소를 통해서 많은 사람의 기여를 통해 소프트웨어가 개발 및 유지,보수된다. 협업을 수월하게 하기 위해 되도록 '컨트리뷰팅 가이드'에 따라서 기여해야 한다. 많은 빅테크 기업도 오픈소스 프로젝트에 많은 기여를 하고 있는 추세이다. 

오픈소스 비지니스 모델이 다 다르다. 따라서 오픈소스를 활용하기 전에 라이센스를 반드시 확인해야한다. 상업적 용도의 사용이 제한되어 있을 수 있다. 

오픈소스 활동을 하는 것은 개발자 커리어에 큰 도움이 된다. 오픈소스 프로젝트에 참여하는 것은  전 세계 여러 사람과 소통하면서 협업을 해볼 수 있는 기회이다. 취업, 글로벌 협업, 자기계발에 도움이 된다. 

[📗 오픈소스가이드북](https://opensource.guide/ko/how-to-contribute/)
(https://joone.net)  
[📗 네이버에서 제공하는 오픈소스가이드북](https://naver.github.io/OpenSourceGuide/book/)  
[📗 만화로 보는 오픈소스 이야기](https://joone.net/)  
[📗 공공기관에서 관리하는 오픈소스 커뮤니티](https://www.oss.kr)  

처음 입문할 때 'Good first issue' 라벨이 붙은 문제를 해결해보자. good first issue 라벨은 입문 개발자를 위해 비교적 해결이 수월한 문제를 표시해 놓은 것이다. 이외에도 코드 뿐만 아니라, 번역, 행사 주최 등도 컨트리뷰션이므로 다양한 방식으로 기여해보자.

[📗 Good First Issue를 모아놓은 사이트](https://goodfirstissue.dev)

국내의 경우 오픈소스 프로젝트 활용을 잘 하는 사람을 우대하는 경향이 있고, 해외 빅테크의 경우 오픈소스 개발자를 채용하는 경우가 늘어나고 있다.

[📗 open source 개발자 채용을 정리해 놓은 곳](https://github.com/t9tio/open-source-jobs)  
[📗 open source 개발자 채용 실시간으로 올라오는 곳](https://www.fossjobs.net)

---

## 오픈소스 개발 과정에서 사용되는 git 명령어 알아보기
  
__오픈소스에 코드레벨에서 기여하는 방식은 다음과 같다.__  
fork(remote) -> clone(local) -> make branch(local) -> push(remote) -> pull request(github 기능)

* fork라도 master branch를 건드리지 않는다. 안그러면 버전이 꼬인다. 반드시 branch 만들어서 작업하기.   
(master에 push 해놓고 pull request를 날렸는데, pull request가 그대로 반영되지 않으면 문제가 생기겠죠?)

* fork된 repository를 계속 동기화 해주어야한다. 동기화 이후에 작업을 해야지 꼬이지 않는다.  
(최신 버전에서 작업을 하는게 당연히 더 좋겠죠?)  

    1. git remote add upstream {upstream_repository}  
    git fetch upstream  
    git checkout master  
    git merge upstream/master  
    git push origin master  

    2. github 웹에서 fetch upstream 기능 활용.


__git 자주쓰는 명령어__  

1. 기여 순위 확인하기
    > git shortlog -sn | nl

2. git commit log를 간략하게 확인하기
    > git log --oneline

3. 특정 폴더 내의 commit log 확인
    > git log --oneline ${folder_name}

4. 특정 시점 내에 commit log 확인
    > git log --online --after=${date} --before=${date}

5. 특정 커밋 변경사항 확인
    > git show ${commit_id}

6. 전체 commit 변경사항과 같이 확인
    > git log -p

7. 특정 파일 commit 사항 확인
    > git blame ${file_name}

8. branch 생성 
    > git branch ${branch_name}

9. branch 바꾸기
    > git switch ${branch_name}

10. commit을 되돌릴 때  
    로컬에서만 쓰임(지양) => 아예 commit이 흔적도 없이 사라지는데 버전이 꼬이게 될 수 있음
    > git reset ${commit_id}    


    협업에서는 반드시 이거만 쓰기 => 기록을 남기면서 commit 한 것을 되돌림
    > git revert ${commit_id}

---

* 이 글은 5월 7일 2022 소프트웨어 마에스트로 오픈소스 SW특강(by 김관영 멘토님)를 바탕으로 작성되었습니다.