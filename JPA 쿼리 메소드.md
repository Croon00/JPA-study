## JPA 쿼리 메소드

```java
@Entity
// 미리 쿼리를 Jpql 혹은 native쿼리를 정의를 해놓고 그 쿼리를 정의할 때 사용한 name을 가지고 look up 해서 사용하는 방법
// entity 이름 뒤에 . 찍고
// 대신 좀 지저분해짐
@NamedQuery(name = "Post.findByTitle", query = "SELECT p FROM Post AS p WHERE p.title = ?1")
//@NamedNativeQuery() 네이티브 쿼리를 사용할 때
```

```java
public interface PostRepository extends JpaRepository<Post, Long> {

    List<Post> findByTitleStartsWith(String title);
    
    // 이 메소드 자체가 네임을 찾을 때 사용
    List<Post> findByTitle(String title);
}
```

```java
@Query("SELECT p FROM Post AS p WHERE p.title = ?1")
    List<Post> findByTitle(String title);
// 그냥 이렇게 쓰는게 편함
```
