---
description: Day 5 Spring Web MVC로 구현
---

# Day 5 Spring Web MVC로 구현

### Annotations

<table><thead><tr><th width="233.33333333333331">Name</th><th width="194">Parameter</th><th>Details</th></tr></thead><tbody><tr><td>@RequestMapping</td><td>path,<br>HTTP method</td><td>어떤 HTTP method로 들어왔더라도 context path가 일치하면 일단 이 컨트롤러or메소드로 끌고 들어옴</td></tr><tr><td>@GetMapping<br>@PostMapping<br>@PatchMapping<br>@DeleteMapping</td><td>path</td><td>Context path도 일치하고 HTTP method도 일치해야 함</td></tr><tr><td>@PathVariable</td><td>path variable 중 사용할 것 지정 (ex: /{id} -> id)</td><td>Path variable을 parameter로 받고자 하는 경우 사용</td></tr><tr><td>@RequestBody</td><td>-</td><td>Parameter로 RequestBody 내용 자체를 받고자 하는 경우 사용</td></tr><tr><td>@ExceptionHandling</td><td>Exception 종류 (Class로 정의한 경우 ~~.class)</td><td>해당 컨트롤러로 리소스 접근하던 중 parameter로 지정한 exception 발생 시 그 메소드를 실행시킴<br>(ErrorHandler class 나중에 따로 구성 가능)</td></tr></tbody></table>

* 컨트롤러에 적힌 @\~\~Mapping annotation이 메소드보다 우선함
  * 컨트롤러에서 이미 @RequestMapping("/posts")가 적용된 경우, 하위 메소드에서는 @GetMapping("/1")이라고만 해도 /posts/1 로 들어오는 요청을 받아서 처리할 수 있음

### 그 외 메모

* Devtools가 load 안되는 경우
  * Setting -> Build \~ -> Complier -> Build Project Automatically를 체크해주기
* ", ', \ 등 기호를 input/output stream으로 온전하게 전송하려면 역슬래시(\\)를 적절하게 사용해주기
* Spring Boot 사용 시 Spring Web MVC에서 정의하는 형태로 resource를 정의함
  * PostController는 post라는 resource에 접근하는 컨트롤러이다!
* Exception Handling method는 기능 구현 메소드들 아래쪽에 분리해서 두자 (찾기 편하도록)
* JAX-RS
  * [https://sarc.io/index.php/development/1159-jax-rs](https://sarc.io/index.php/development/1159-jax-rs)
  * Java platform에서 경량화된 형태로 REST 방식 web application을 표현할 수 있는 API
  * Resource 기반으로 web application을 정의하고 관리할 때에 용이하게 사용할 수 있음
