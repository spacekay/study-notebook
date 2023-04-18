---
description: Day 4 Java HTTP Server
---

# Day 4 Java HTTP Server

* Java에는 Non-blocking IO HTTP Server를 구현할 수 있는 object가 있음
  * HttpServer : ServerSocket / Socket 쓰는 것에 비하면 훨씬 고수준 서버

### **NIO**(Non-blocking IO)란?

* 참고 블로그 : [https://etloveguitar.tistory.com/140](https://etloveguitar.tistory.com/140)
* Blocking 이란?
  * 어떤 함수를 실행하든 CPU가 다음 작업을 할 수 없도록 process를 block함
  * CPU latency에 비해 모든 종류의 IO는 훨씬 느림
  * Application에서 IO를 진행하는 동안 CPU process가 실제 성능보다 오랜 시간동안 block 되어 있는 경우 존재 -> 자원 비효율
  * Non-Blocking 이란?
    * Application의 실제 IO 작업이 끝나기 전에 Application 함수의 IO 호출은 빠르게 return함
    * 이 경우 CPU process는 다음 작업을 바로 이어서 할 수 있고, 그동안 IO는 별개로 일어남\
      (이미 요청은 실행되었으므로 호출 이후 CPU가 다른 일을 한다고 진행 중인 IO가 영향을 받거나 하지 않음)
    * 대신에, 다음 작업을 진행하는 중에 Appliction은 IO 완료 여부를  kernal에  중간중간 확인함(System Call)
    * 완료된 것을 확인한 경우 Application에서도 IO 작업을 완료 처리
  * Sync / Async와의 차이
    * Blocking / Non-Blocking : IO 함수의 작업이 아직 끝나지 않았으나, CPU는 작업이 가능할 때의 다음 작업 진행 여부 차이
    * Sync / Async : Sync는 앞선 작업 여부를 커널에 계속 확인하지만, Async는 앞선 작업은 백그라운드에서 실행하고 새로운 작업을 진행할 수 있음. 이전 작업이 끝나면 커널로부터 응답을 받음

### **Java HTTP Server** 구현 과정

1. Server를 HttpServer 객체로 구현
   1. HttpServer는 생성자에  InetSocketAddress, backlog를 받음
   2. InetSocketAddress는 생성자에 호스트 주소 또는 포트 번호와 HttpHander를 받음
   3. 내장 Error Handler 존재!
      * 일반적인 ServerSocket / Socket으로 구현하던 때와는 달리, 서버에 아무것도 넣지 않고 띄워도 404 Not Found 화면을 띄울 수 있음
   4. HttpHandler.createContext();로  Path별 context 설정
   5. HttpHandler는 method가 하나뿐인 interface이므로, lambda function으로 객체 생성 가능
   6. HttpHandler 내부에서는, HttpExchange 객체에 request 또는 response 내용을 담아 read하고 write함
   7. Request
      * HTTP Method, path, header, body를 확인함
      * byte array를 String으로 변환 시, new String(bytes);의 형태로 String 객체를 생성해야 함 (toString(); 쓰는거 아님)
   8. Response
      * Response header에는 Status code와 content-length가 필수
        * content-length는 byte array의 length로 전달
   9. 이후 httpServer.start(); 해두면 서버는 계속 열려있음. (아예 닫는 method는 발견하지 못함)

