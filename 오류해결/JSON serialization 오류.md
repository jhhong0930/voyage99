Layz 로딩으로 인한 JSON 오류

외래키로 다른 객체를 참조하여 조회시 JSON으로 받아오는 과정에서 Serialization이 안되는 문제

Lazy 로딩은 데이터베이스에서 Obejct를 가져올 때 맵핑되어있는 다른 Object를 가져오지 않는다

Jackson은 외래키로 참조되어있는 객체를 serialize 하려고 하지만 정상적인 Object가 아닌 JavaassistLazyInitializer 때문에 실패하게된다

```java
// application 파일 설정
Spring:
  jackson:
    serialization:
      fail-on-empty-beans: false

// 해당 Object에 (외래키로 참조하는 객체) Lazy 로딩을 지우거나 @JsonManagedReference
@ManyToOne(fetch = FetchType.LAZY)
@JsonManagedReference
@JoinColumn(name = "user_id")
private User user;

// 그 반대쪽에 @JsonBackReference
@OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
@JsonBackReference
private List<Recipe> recipeList;

// 각 연관관계 매핑별 기본 fetch 타입
// OneToMany: LAZY
// ManyToOne: EAGER
// OneToOne: EAGER
// ManyToMany: LAZY
```

