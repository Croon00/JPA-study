## Spring data jpa에 쿼리 데이터 사용할 때 팁

```java
// 원래 채번 했어야 되는데 이제 네임드 파라미터를 사용가능
    // title 을 @Query에 넣기
    @Query("SELECT p FROM Post AS p WHERE p.title = :title")
    List<Post> findByTitle2(@Param("title")String keyword, Sort sort);

    // entity이름을 {entityName}으로 사용 --> 참조해서 사용 가능 JpaRepository<Post, Long> 여기 있는 Post 이거
    // @Entity("posts") 로 바꾸더라도 여기서 바꿀 필요가 없어지는 장점이 있다.
    @Query("SELECT p FROM #{#entityName} AS p WHERE p.title = :title")
    List<Post> findByTitle3(@Param("title")String keyword, Sort sort);
```
