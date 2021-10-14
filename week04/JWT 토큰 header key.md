jwt token을 header에 담아서 보낼 때 key값을 JwtTokenProvider에서 프론트가 보내준 값과 일치시켜야 한다

```java
public String resolveToken(HttpServletRequest request) {
    return request.getHeader("X-AUTH-TOKEN");
}
```

