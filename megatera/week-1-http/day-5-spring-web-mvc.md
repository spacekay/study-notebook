---
description: Day 5 Spring Web MVC
---

# Day 5 Spring Web MVC

### Spring에 대한 기본 이해

* [https://docs.spring.io/spring-framework/docs/current/reference/html/overview.html#overview](https://docs.spring.io/spring-framework/docs/current/reference/html/overview.html#overview)
  * Coverage
    * Servlet API ([JSR 340](https://jcp.org/en/jsr/detail?id=340))
    * WebSocket API ([JSR 356](https://www.jcp.org/en/jsr/detail?id=356))
    * Concurrency Utilities ([JSR 236](https://www.jcp.org/en/jsr/detail?id=236))
    * JSON Binding API ([JSR 367](https://jcp.org/en/jsr/detail?id=367))
    * Bean Validation ([JSR 303](https://jcp.org/en/jsr/detail?id=303))
    * JPA ([JSR 338](https://jcp.org/en/jsr/detail?id=338))
    * JMS ([JSR 914](https://jcp.org/en/jsr/detail?id=914))
    * as well as JTA/JCA setups for transaction coordination, if necessary.
  * Design Philosophy
    * Provide choice at every level.
    * Accommodate diverse perspectives.
    * Maintain strong backward compatibility.
    * Care about API design.
    * Set high standards for code quality.

### **MVC Archtecture Pattern**

* 구성 요소
  * View : 표현 (외부로 드러남)
  * Controller : 입력 (by client, 비즈니스 로직 호출 역할만 함)
  * Model : 그 외 모든 것 (비즈니스 로직 포함)
* MVC 패턴을 쓰는 이유
  * 관심사의 분리
    * Model에 핵심 비즈니스 로직이 다 들어가므로, Model에만 집중하여 관리할 수 있음
* **Spring Web MVC**의 특징
  * UI Layer에만 MVC 패턴을 적용하고, Model 부분에서 그 외에 다양한 패턴을 적용함\
    (추후 배우게 될 Hexagonal Archtecture 포함.. 어려운거 같아 일단 나중에 깊게 보기로 함)
    * Model이라는 Map과 유사한 Interface를 제공하여 개발할 수 있음음
* \+  **Active Record Pattern**
  * Rails가 채택
  * Model - DB를 1대1 매치\
    (쿼리를 사용할 필요가 없음. 단, DB 접근이 불필요하게 많아질 수 있음)

### Spring Web MVC 구현

* [https://start.spring.io/](https://start.spring.io/)
  * 반드시로컬 IDE에서 initialize할 필요는 없음
  * Spring Web + devtools 를 꼭 dependency에 추가하자.
  * Spring 3.0 이상 부터는 Java 17 이상만 지원하므로 참조
* 구현 과정
  * 다운로드 받은 압축파일을 풀어서 안에 들어간 다음 -> idea .
  * IntelliJ에서 devtools를 작동하려면 초기에 프로젝트 단위로 설정할 것이 많음
    * [https://rooftoproom-whale.tistory.com/21](https://rooftoproom-whale.tistory.com/21)
  * 초기 build가 되고 나면 bootRun command로 Gradle run
  * appliaction.properties 에서 포트 넘버 부여 (port.number=\~) <- 정확한게 맞는지 다시 확인
  * Controller 작성 (@RestController, @GetMapping 활용)
* **처음 Annotation은 항상 잘 뜯어보기!**
  * @RestController&#x20;
    * @Target(ElementType.TYPE)
    * @Retention(RetentionPolicy.RUNTIME) <- 서버 작동 중에 유효함
    * @Documented
    * @Controller
    * @ResponseBody <- Response body 내용을 직접 return하는 controller (일반 controller처럼 path를 return하는 것이 아님)
  * @GetMapping&#x20;
    * @Target(ElementType.METHOD) <- Request의 HTTP method로 작동
    * @Retention(RetentionPolicy.RUNTIME) <- 서버 작동 중에 유효함
    * @Documented&#x20;
    * @RequestMapping(method=RequestMethod.GET) <- 상위 Annotation
  * @ResponseStatus&#x20;
    * Response status를 지정하여 return 가능\
      (정제되지 않은 서버 에러를 뿜어서 client가 통신 상황을 파악하기 어렵게 만들기보다는, 스테이터스를 지정하여 return하는 것이 HTTP 적으로 더욱 타당함)
