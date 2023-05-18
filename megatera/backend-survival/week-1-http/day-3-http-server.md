---
description: Day 3 HTTP Server
---

# Day 3 HTTP Server

### **HTTP Server의 통신 순서**

* **Listen** (Server on)
* **-> Accept**
* **-> Send & Receive**
* **-> Close** (Server off)
* 항상 클라이언트로부터 Request를 받고, 클라이언트로 Response를 보냄
* 보통 서버는 최초 요청 받기용 소켓(ServerSocket)과, 특정 클라이언트 대응용 소켓(Socket)을 따로 만든다.
* Blocking : 서버가 listen하는동안 client로부터의 I/O를 기다리는 작업\
  (파일 읽기-쓰기도 블로킹의 일종. 소켓 만들기-닫기가 그와 같다는 점을 이해하자.)
  * Blocking 작업 중의 자원 비효율을 개선하기 위하여, Multithread, Async, event 기반 처리가 필요

### 케이스별 사용 Class

* 대부분 Client와 용례가 비슷
* ServerSocket : Device의 IP에서 특정 Port를 열어두고 Client의 Connect 요청을 기다림
  * Socket class를 상속한 것이 아님
  * 생성자에 포트 번호와 backlog 수치를 기입
    * backlog : 해당 서버의 Client queue(대기줄)에 세울 최대 Client 수\
      (기본값은 50)
* Response를 보내줄 때에도 기본적인 헤더 작성을 하는 것이 좋음
  * Content-Type : text/plain와  text/html는 브라우저에서 다른 폰트로 출력함
  * Content-Length : 지정해주지 않으면 클라이언트는 body의 끝을 알 수 없음
    * 일반 Text length로 작성하면 안되고, byte array로 변환 후 해당 array의 length를 작성하여야 함
    * Content-Length 작성하지 않는 경우, 가급적 body 가장 아래에 줄바꿈 문자(\n)를 추가해 주자.
