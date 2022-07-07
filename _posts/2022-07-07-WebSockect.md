---

title: Web Socket이란 무엇일까?
categories: 
    - WEB
    - network
tags:
    - TIL
    - Concept
 
---

# 웹소켓이란?

웹소켓은 클라이언트와 서버 간 양방향 통신에 사용되는 프로토콜이다. 트레이딩 앱, 채팅 앱, 게임 앱 등 실시간 성 데이터가 필요한 경우 쓰인다.

<br>

# 핸드쉐이크
웹소켓의 핸드쉐이크는 HTTP의 방식을 바탕으로 이루어진다.

클라이언트는 다음과 같이 요청한다.
> GET /chat HTTP/1.1  
Host: server.example.com  
Upgrade: websocket  
Connection: Upgrade  
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==  
Sec-WebSocket-Protocol: chat, superchat  
Sec-WebSocket-Version: 13  
Origin: http://example.com  


서버는 다음과 같이 응답한다.
> HTTP/1.1 101 Switching Protocols  
Upgrade: websocket  
Connection: Upgrade  
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=  
Sec-WebSocket-Protocol: chat  

[😀 요청, 응답 헤더 예시가 있는 위키피디아 글](https://ko.wikipedia.org/wiki/%EC%9B%B9%EC%86%8C%EC%BC%93)

서버가 클라이언트의 웹소켓 통신 요청을 받아들였는지를 확인하기 위해 응답헤더의 Sec-WebSocket-Accept 값을 활용한다. 서버는 클라이언트로부터 받은 Sec-WebSocket-Key와 "258EAFA5-E914-47DA-95CA-C5AB0DC85B11" 문자열을 concate 한 후 SHA-1로 해쉬한다. 그리고 base64로 인코딩한 값을 Sec-WebSocket-Accept에 넣어서 응답한다. 

>  The |Sec-WebSocket-Accept| header field indicates whether
   the server is willing to accept the connection.  If present, this
   header field must include a hash of the client's nonce sent in
   |Sec-WebSocket-Key| along with a predefined GUID.


```javascript
const crypto = require('crypto');

const SecWebSocketKey = "x3JJHMbDL1EzLkh9GBhXDw==";
const WebSocketGUID = "258EAFA5-E914-47DA-95CA-C5AB0DC85B11";
const concated = SecWebSocketKey + WebSocketGUID;
const SecWebSocketAccept = crypto.createHash('sha1').update(concated).digest('base64');

console.log(SecWebSocketAccept) // HSmrc0sMlYUkAGmm5OPpG2HaGWk=  
```

[😁 웹소켓 스펙이 담긴 rfc 문서](https://www.rfc-editor.org/rfc/rfc6455)

<br>

# 데이터 교환
웹소켓에서 교화되는 데이터는 'frame'이라고 불린다. 웹소켓이 연결되면 데이터 프레임을 서버, 클라이언트 양쪽에서 모두 자유롭게 보낼 수 있다. 핸드쉐이크 이후부터 서버나 클라이언트는 ping 패킷을 보낼 수 있다. ping 패킷의 수신자는 최대한 빠르게 pong 패킷을 보내야한다. 그렇지 않으면 수신자가 더 이상 통신을 하지 않고 있다고 판단하게 된다. 이러한 방식으로 서버는 클라이언트가 살아있는지 확인 할 수 있다.

서버나 클라이언트 어느 한 쪽에서 연결을 종료하기 전까지 연결은 계속 유지된다. 

[🤠 데이터 프레임이 어떻게 구성되어있는지 잘 설명해주는 글](https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API/Writing_WebSocket_servers#%EB%A7%88%EC%8A%A4%ED%82%B9%EB%90%9C_payload_%EB%8D%B0%EC%9D%B4%ED%84%B0_%EB%94%94%EC%BD%94%EB%94%A9%ED%95%98%EA%B8%B0)
