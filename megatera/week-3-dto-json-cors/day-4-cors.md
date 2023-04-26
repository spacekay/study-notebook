---
description: Day 4 CORS
---

# Day 4 CORS

### Same-Origin Policy

* 웹 브라우저(Client side)에서 적용하는 보안 정책 -> F/E와 소통 시 고려
  * [https://kimkoungho.github.io/security/same-origin-policy/](https://kimkoungho.github.io/security/same-origin-policy/)
  * F/E에서 리소스를 요청할 때, F/E의 **프로토콜+도메인(호스트)+포트 정보**가 Origin이 됨
  * F/E는 B/E에 URL(Host + Context Path)로 리소스를 요청함
  * B/E는 받아서 응답해줌 (서버에서는 응답 완료!)
  * 웹 브라우저에서 B/E의 응답을 받았는데 그 응답의 Origin 정보가 현재 Origin과 다르면 보안 문제(해킹 등)로 인식함
* 얻으려는 리소스를 요청한 B/E 호스트가 **현재 페이지**의 F/E 호스트와 다르면 접근 불가하게 함
* ex> F/E : http://localhost:3000\
  &#x20;      B/E : http://localhost:8080 -> 포트 번호가 다르므로 다른 Origin

```
GET /posts HTTP1.1 /* 어차피 path는 Host나 Origin과 따로 정의하므로 상관 없음 */
Host: http://localhost:8080 /* REST API */
Origin: http://localhost:3000 /* Frond-end */

// 이 경우 Client에서CORS error 발생
```

### 과거 Same-Origin Policy를 우회했던 방식 -> \<script> 활용

* \<script> 태그로 받은 JavaScript 코드로 실행한 내용은 동일 출처를 따지지 않음
  * 서버로부터 callback 함수 코드를 받아 그것을 처리한 결과를 출력함

```javascript
<script>
windows.success = (data) => {
    // 서버에서 받은 데이터를 처리하기 위한 코드가 돌아감
    console.log(data);
}
</script>
<script src="http://localhost:8080/posts?callback=success"></script>
// 서버에서는 callback parameter 값을 success로 받고, 그에 맞는 함수의 JavaScript 코드를 전송
// DTO 형태로 JSON을 전송하는 것이 아닌 JavaScript 코드를 B/E에서 보내주어야 함
```

* 이 경우 문제점
  * CSRF  (Cross-Site Request Forgery)
    * 외부에서 script를 실행시켜 클라이언트를 공격할 수 있음
    * (이를 방어하기 위해 API 호출 시 CSRF 토큰을 사용하기도 하나.. 근본적인 해결책은 아님)

### CORS

* Cross-Origin Resource Sharing  -> 서로 다른 오리진이 리소스를 나눔
  * 웹 브라우저에서는 이를 잠재적인 보안 위협으로 보고 엄격하게 관리함
* 개발 중 CORS 에러 발생 시 해결 방법: Backend 응답 헤더에 'Access-Control-Allow-Origin' 속성을 포함시킴
  * 서버에서 '여기에서 요청했다면 괜찮다'고 웹 브라우저에 알려주는 효과
* Spring Web에서의 CORS 구현

1. HttpServletResponse 객체를 개별 메소드의 parameter로 받아서 addHeader() 해주기

```java
@GetMapping
public String getList(
    HttpServletResponse response
) {
    response.addHeader("Access-Control-Allow-Origin", "http://localhost:3000")
    return "OK\n";
}
```

2. @CrossOrigin annotation을 개별 메소드 혹은 Controller Class마다 추가

```java
@CrossOrigin("http://localhost:3000")
@RestController
public class PostController {
    ...
} 
```

3. WebMvcConfigurer Interface를 구현하는 클래스 만들기
   1. [https://w97ww.tistory.com/82](https://w97ww.tistory.com/82)

```java
@Bean
@Configuration
public Class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://localhost:3000")
                .allowedOrigins("https://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS");
    }
}
```

* 관련 팁
  * Origin 정의할 때 \*(wildcard character)를 사용하면 모든 origin을 허용함
  * @CrossOrigin 사용 시 parameter를 안 넣으면 자동으로 \* 적용
  * WebMvcConfigurer에서 allowedOrigins()를 쓰거나, HttpServletResponse에서 addHeader()를 쓰는 경우에는 \* 하나는 꼭 입력해 주어야 함 (아무것도 안쓰는 것 X)
