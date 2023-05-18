---
description: Day 2 HTTP Client
---

# Day 2 HTTP Client

### **TCP/IP 통신**

* Internet Protocol Suite (컴퓨터 인터넷은 TCP/IP 통신을 활용함)
  * 송신자 - 수신자 간 연결 보장을 위하여 Socket을 사용함
* Berkely Socket (API)
  * Socket 프로그래밍을 위한 API
* (Network) Socket
  * 각 네트워크 프로세스간 통신의 종착점 (전화기의 스피커와 같음)
  * Socket은 File과 유사하게 다룰 수 있음
    * 실제로 UNIX에서는 socket을 file descriptor의 일종으로 사용
    * 예전에 Ubuntu에서 socket 관련 설정을 할 때, 파일 시스템에서 조회 가능했던 것으로 기억함
  * Java의 IO Stream으로 다루기 편함 (개념이 유사함)
* TCP 통신 순서 (전화 받기와 비슷)
  * **Listen**
  * **-> Connect**
  * **-> Accept**
  * **-> Send & Receive (반복)**
  * **-> Close** (Client는 Close 사실을 Receive를 통해 알 수 있음)
* **HTTP Client의 통신 순서**
  * Connect
  * \-> Send & Receive
  * \-> Close(Receive)
  * 항상 서버로부터 Respose를 받고, 서버로 Request를 보냄냄

### Client Socket 개발 과정 꿀팁 메모

* Terminal에서 idea . -> IntelliJ에서 바로 프로젝트 오픈 가능
* Ctrl + Shift + T : 일반 앱 클래스에서 테스트 클래스로, 테스트 클래스에서 일반 앱 클래스로 바로 이동 가능
* Alt + Enter : 빨간줄 쳐졌을 때 빠른 대응을 위한 선택지 띄우기 가능
* 프로젝트 새로 만들 때마다 Actions On Save 설정하기 : import 최적화만 해도 큰 도움
* 케이스별 사용 Class
  * (Client) Socket : Socket
    * Closeable이어서, try-with-resources 가능\
      (try문 안에서만 해당 객체를 사용하고 try문 끝나면 자동 close 가능)
  * **Byte Array**로 데이터 송수신 : InputStream, OutputStream <- Stream은 Byte 단위로 !
    * 들어온 순서 그대로 데이터를 받아들이면 됨\
      (대신 inputStream.read() 등으로읽은 값을 변수로 저장하지 않으면 날아감)
    * 좀더 세부 절차가 번거로움
      * 미리 정의한 Byte Array 크기가 Chunk(작업단위)의 크기가 됨
      * Chunk보다 큰 데이터가 한번에 들어오면 여러번 나누어서 값을 읽어들이게 할 수 있음
  * **CharBuffer**로 데이터 송수신 : InputStreamReader(Reader class 상속), OutputStreamWriter(Writer class 상속)
    * 좀더 절차가 간소화(Array 사이즈 조절 절차 X)되나, charBuffer.flip(); 을 넣어주어야 사람이 볼 수 있는 데이터 송수신 가능
    * CharBuffer도 Chunk 크기를 정의할 수 있으며, Chunk보다 큰 데이터가 한번에 들어오면 여러번 나누어서 값을 읽어들이게 할 수 있음
