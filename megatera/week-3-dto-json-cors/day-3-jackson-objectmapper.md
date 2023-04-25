---
description: Day 3 Jackson ObjectMapper
---

# Day 3 Jackson ObjectMapper

### Jackson ObjectMapper

* Java에서 DTO(데이터)를 JSON(문자열)으로, JSON(문자열)을 DTO(데이터)로 변환하기 위해 사용하는 라이브러리
* Spring에서 기본 제공하고 있음\
  (ObjectMapper 객체를 별도 선언하지 않아도 내부 클래스간 데이터 전송 시 자동으로 read/write 매핑 가능)
* JSON 스키마를 Java 코드로 표현하기

#### JSON Schema

```json
// JSON 스키마로 사람을 표현
{
    "id": 94,
    "name": "spacekay",
    "group": "backend survival",
    "term": 3
}
```

#### Java code (Class)

```java
// Java Class로 사람을 표현
public class People {
    private Long id = 94L;
    private String name = "spacekay";
    private String group = "backend survival";
    private Integer term = 3;
}

// 사람에 대한 데이터를 전달하는 DTO 정의하기
public class PeopleDto {
    private Long id;
    private String name;
    private String group;
    private Integer term;
    
    // 생성자는 필수
    public PeopleDto() {
    }
    
    public PeopleDto(Long id, String name, String group, Integer term) {
        this.id = id;
        this.name = name;
        this.group = group;
        this.term = term;
    }
    
    // Getter도 필수
    public Long getId() {
        return this.id;
    }
    
    public String getName() {
        return this.name;
    }
    
    public String getGroup() {
        return this.group;
    }
    
    // 네이밍룰 조율 등을 위해 필요한 경우 @JsonProperty annotation 활용 
    @JsonProperty("GroupTerm") 
    public Integer getTerm() {
        return this.term;
    }   
 }
```

## 구현 과정 관련 메모

### Spring DI&#x20;

* Dependency Injection
* Spring 부팅 시 @Component annotation이 붙은 객체들을 모두 스캔함\
  (주로 @RestController 등 @Controller 객체들)
* 이때 Component들의 객체 의존성을 파악하여 해당 객체들에 선언된 객체들을 외부(스프링 환경 - IoC Container라고 부름)에서 먼저 생성한 뒤, 주입받는 객체들을 나중에 만들면서 생성자를 통해 주입하여 줌
  * IoC : Inversion of Control -> 제어의 역전. 객체가 자신의 필드를 구성하는 객체를 직접 만들지 않고, 외부(스프링)에서 주입해 주는 것을 그대로 사용함.
* 객체를 주입한다 -> Controller 등 특정 객체 내에서 인스턴스를 하나 넣어준다\
  (해당 객체의 내부에만 종속된 객체를 만들어 두는 것이 아님)
  * 단, 이런 경우 DI 에러가 발생하며 스프링 부팅 자체가 안됨
    * 컨트롤러안에 서비스 객체를 넣었는데, 그서비스 안에 또 컨트롤러 객체를 넣음\
      \-> Circular Dependencies (예전 프로젝트 할 때 종종 경험함...)
* 의존성 주입된 객체는 IoC Container 내에서 계속 **싱글톤 패턴**으로 관리 <- 자원 효율성 증대
  * 같은 Repository 객체를 10개 서비스에서 써도, 해당 객체는 한번만 생성하여 10군데에 넣어줌. 10개 서비스는 결론적으로 하나의 객체를 사용하는 것임.

### Java에서 JSON 처리하기

* ObjectMapper 객체 사용하기
  * 컨트롤러에 ObjectMapper 객체를 DI해서 직접 사용할 수 있음
  * DTO to JSON : objectMapper.writeValueAsString()
  * JSON to DTO : objectMapper.readValue()
* 그냥 스프링에게 맡기기
  * 컨트롤러 상 메소드의 파라미터나 리턴을 Dto 객체로 정의하면, 파라미터를 받거나 리턴할 때 Dto 객체의 내용을 자동으로 JSON으로 직렬화하여 F/E에게 전송함
* DTO Package 관리 방식
  * 모든 필드를 다 가진 DTO로만 관리하기보다는, 기능별로 필요한 필드만을 가진 DTO들을 하나하나 분리하여 관리하는 것을 추천\
    (ex: LoginDTO, UserDTO, PasswordDTO, EmailDTO 등...)
  * 프로젝트 전체에서 전역으로 사용 가능한 DTO로 정의해도 되고, Controller 등 특정 영역에서만 사용 가능한 DTO를 따로 정의해도 됨\
    (필요에 따라, 또는 개인 취향에 따라 분리 가능함. 내 취향은 전역에 두는 것)
