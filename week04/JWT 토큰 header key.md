토큰을 이용하게 되면 쿠키에 토큰을 담아 header에 실어서 보내게 된다, front에서는 이 토큰을 받아서 로그인 정보를 이용하여 로그인을 하고 매 권한이 필요한 요청마다 토큰을 담아서 같이 보내게 된다.

```java
// JwtTokenProvider

private String secretKey = "시크릿 키";
// 프론트와 일치시켜야 하는 토큰 유효시간
private Long toemValidTime = 24 * 60 * 60 * 1000L;

// screckey를 base64로 인코딩
@PostConstruct
protected void init() {
    secretKey = Base64.getEncoder().encodeToString(secretKey.getBytes());
}

// JWT 토큰 생성
public String createToken(String userPk) {
    Claims claims = Jwts.claims().setSubject(userPk); // JWT payload 에 저장되는 정보단위

    Date now = new Date();

    return Jwts.builder()
        .setClaims(claims) // 정보 저장
        .setIssuedAt(now) // 토큰 발행 시간 정보
        .setExpiration(new Date(now.getTime() + tokenValidTime)) // set Expire Time
        .signWith(SignatureAlgorithm.HS256, secretKey)  // 사용할 암호화 알고리즘과
        // signature 에 들어갈 secret값 세팅
        .compact();
}

// 토큰에서 회원 정보 추출
public String getUserPk(String token) {
    return Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token).getBody().getSubject();
}

// JWT 토큰에서 인증 정보 조회
public Authentication getAuthentication(String token) {
    UserDetails userDetails = userDetailsService.loadUserByUsername(this.getUserPk(token));
    return new UsernamePasswordAuthenticationToken(userDetails, "", userDetails.getAuthorities());
}

// Request의 header에서 토큰 값 가져오기 (key값을 프론트와 일치시켜야 한다)
public String resolveToken(HttpServletRequest request) {
    return request.getHeader("X-AUTH-TOKEN");
}

// 토큰의 유효성 + 만료일자 확인
public boolean validateToken(String jwtToken) {
    try {
        Jws<Claims> claims = Jwts.parser().setSigningKey(secretKey).parseClaimsJws(jwtToken);
        System.out.println(claims); // JWT 토큰(클라이언트에서 보낸)이 잘 들어오는지 검증
        return !claims.getBody().getExpiration().before(new Date()); // expire시간이 되지 않았다면 True
    } catch (Exception e) {
        return false;
    }
}
```

