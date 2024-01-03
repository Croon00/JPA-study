## 스프링 데이터 Common 기본 리포지토리 커스터마이징(모든 Entity)

모든 리포지토리에 공통적으로 추가하고싶은 기능이 있거나 덮어쓰고 싶은 기능이 있다면

```java
// 중간에 사용되는 리포티터리는 꼭 @NoRepositoryBean 추가
@NoRepositoryBean
public interface MyRepository<T, ID extends Serializable> extends JpaRepository<T, ID> {
// 해당 entity가 persistContext에 들어 있냐 없냐
    boolean contains(T entity);

}
```

```java
// 가장 윗단에있는, 가장 많은 기능을 가진 simpleJpaRepository
public class SimpleMyRepository<T, ID extends Serializable> extends SimpleJpaRepository<T, ID> implements MyRepository<T, ID> {

    private EntityManager entityManager;
// 생성자를 추가해주어야 한다, SimpleJpaRepository에 super로 할때 2개의 인자를 꼭 넘겨아 하기 때문에
    public SimpleMyRepository(JpaEntityInformation<T, ?> entityInformation, EntityManager entityManager) {
        super(entityInformation, entityManager);
// 빈으로 받는게 아니라 여기서 전달 받기
        this.entityManager = entityManager;
    }

// persistentContext 에 있는지 없는지 확인
    @Override
    public boolean contains(T entity) {
        return entityManager.contains(entity);
    }
}
```

```java
@EnableJpaRepositories(repositoryBaseClass = SimpleMyRepository.class)
// spring 실행 클래스에
```

```java
// 이제 myRepository를 사용하면 된다.
public interface PostRepository extends MyRepository<Post, Long> {
}
```
