### REST?

- Representational state Transfer - 효율적, 안정적이며 확장가능한 분산시스템을 가져올 수 있는 소프트웨어 아키텍처 디자인 제약의 모음, 이러한 제약들을 준수 하였을 때 그 시스템은 RESTful 하다 라고 할 수 있다
- REST의 구성 요소
  - 자원(Resource) - URI
  - 행위(verb) - HTTP METHOD
  - 표현(Representations)
- REST는 URI를 통해 **자원** 을 표시하고, HTTP METHOD를 이용하여 해당 자원의 행위를 정해주며 그 결과를 받는 것을 말한다

### REST의 특징

- Uniform Interface: HTTP 표준만 따른다면 특정 언어나 플랫폼에 종속되지 않고 사용할 수 있다
- Stateless: 서버는 각각의 요청을 완전히 다른 것으로 인식하고 처리한다, 이전 요청이 다음 요청 처리에 연관이 되면 안된다
- Cacheable: HTTP의 기존 웹 표준을 그대로 사용하기 때문에 HTTP가 가진 캐싱 기능 적용이 가능하다
- Self-descriptiveness: REST API 메시지만 보고도 쉽게 이해할 수 있는 자체 표현 구조로 되어있다
- Layered System: 계층화 클라이언트는 REST 서버는 다중 계층으로 구성될 수 있으며 로드 벨런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 둘 수 있다
- 자원이 있는 쪽을 Server라 하고 자원을 요청하는 쪽이 Client가 된다

### REST API란?

- 이러한 REST 기반의 규칙들을 지켜서 설계된 API를 Rest API 또는 Restful API 라고 한다
- REST API 설계 규칙
  - URI는 정보의 자원을 표현해야 하고 이름은 동사보다는 명사를 사용한다
  - URI는 자원을 표현하는데 중점을 두어야 하기 때문에 행위에 대한 표현이 들어가면 안된다
  - 자원에 대한 행위는 HTTP METHOD로 표현한다
  - '/'는 계층 관계를 나타내는데 사용한다
  - 불가피하게 긴 URI를 사용하게 될 경우 하이픈(-)을 사용하고 언더바(_)는 사용하지 않는다
  - URI 경로에는 대소문자에 따라 각자 다른 리소스로 인식하기 때문에 소문자를 사용한다
  - 파일 확장자는 URI에 포함하지 않는다