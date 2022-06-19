---

title: 도커에서 새로운 컨터네이너 만드는 법
date: 2022-6-19
categories: 
    - docker
tags:
    - how to
    
---

컨테이너만 생성하고 실행하지 않을 때
```properties
docker create [OPTIONS] IMAGE [COMMAND] [ARG...] 
```

컨테이너를 생성하고 실행까지 할 때
```properties
docker run [OPTIONS] IMAGE [COMMAND] [ARG...] 
```

docker run은 docker create 후 컨테이너를 실행하는 것으로 대부분의 options를 docker create와 공유한다.

---

### 기본적으로 도커 컨테이너 생성하는 방법

```properties
docker run -v HOST_PATH:CONTAINER_PATH -it --platform=linux/amd64 IMAGE /bin/bash
```

__OPTIONS__  
* -v: 호스트 컴퓨터의 특정 디렉토리를 컨테이너의 지정된 경로에서 공유한다.  
* -it: -i 와 -t 옵션을 한 번에 쓴 것으로, 컨테이너가 명령을 수행하고 있지 않아도 계속 실행되게 한다.  
* --platform: 컨테이너의 ISA를 지정한다. 서버의 경우 거의 amd64 ISA를 쓰기 때문에 이를 맞춰주는 것이 좋다.  


__COMMAND__  
* /bin/bash : 컨테이너의 bash로 들어간다.




