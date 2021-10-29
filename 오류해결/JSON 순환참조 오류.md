순환 참조 문제

```java
// application 파일 설정
  jackson:
    serialization:
      fail-on-empty-beans: false
          
// @ManyToOne에 @JsonManagedReference 추가
// @OneToMany에 @JsonBackReference 추가
```

