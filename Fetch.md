## Fetch

연관 관계의 엔티티를 어떻게 가져올 것이냐?

(session.get을 이용하면 조회하기 —> hibernate api)

지금(Eager), 나중에 (Lazy)

1. @OneToMany의 기본 값은 Lazy
2. @ManyToOne 의 기본 값은 Eager

ex) post(게시판) 이 있고, comment(댓글) 이 있을 때 게시판만 조회할 때 게시판마다 댓글을 모두 다 같이 조회하면 불필요 할 수 있다.

post에는 comment가 OneToMany임으로 여기서 comment에 fetch 타입이 기본이 Lazy인데 이것을 Eager로 바꾸면 outer join을 통해서 comment까지 한 번에 조회하게 된다.

 post.getComment().forEach(c → {

sysout c.getComment()로 하여도

lazy인 상태에서 comment를 전부 조회한다

}

### fetch 조인을 통한 N + 1해결 법

[JPA 페치(Fetch) 전략 - 즉시 로딩(EAGER)과 지연 로딩(LAZY)](https://rutgo-letsgo.tistory.com/199)

[[JPA] 일반 Join과 Fetch Join의 차이](https://cobbybb.tistory.com/18)
