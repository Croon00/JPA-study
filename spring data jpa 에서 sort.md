## spring data jpa 에서 sort

```java
@Query("SELECT p FROM Post AS p WHERE p.title = ?1")
    // 정렬 옵션 추가하려면 파라미터에 sort 추가 이 문자열ㅇ느 반듣이 property거나 alias만 올 수 있다.
    List<Post> findByTitle(String title, Sort sort);

List<Post> all = postRepository.findByTitle("Spring", Sort.by("title)"));

// 함수를 호출 하려면 이렇게 사용 가능
 List<Post> all2 = postRepository.findByTitle("Spring", JpaSort.unsafe("LENGTH(title)"));
```
