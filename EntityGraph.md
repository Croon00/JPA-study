## EntityGraph

```java
@NamedEntityGraph(name = "Comment.post", attributeNodes = @NamedAttributeNode("post"))

// 기본 fecht 모드가 eager이기 때문에 comment 가져올때 post도 가져온다.
    // lazy 타입으로 하면 post를 볼 때 가져온다.
    @ManyToOne(fetch = FetchType.LAZY)
    private Post post;
```

```java
public interface CommentRepository extends JpaRepository<Comment, Long> {

    // 여기서 연관관계들을 어떻게 쓸것인지 커스텀하게 만들어 쓰기
    // post타입만 설정했지만 comment의 기본 값들은 eager로 가져온다.
    // LOAD등을 쓰면 post는 무조건 eager이고 나머지 기본 값들의 설정을 다르게 주는 것이다.(설정 안된 값들을 전부 lazy)
    // 우리가 연관관계 설정한 모드를 eager로 가져온다 --> 기본적인것
    @EntityGraph(value = "Comment.post")
    Comment getById(Long id);

// 이렇게 쓸 수도 있다.
@EntityGraph(attributePaths = {"post"})
    Optional<Comment> getById2(Long id);
}

```
https://wonin.tistory.com/496
