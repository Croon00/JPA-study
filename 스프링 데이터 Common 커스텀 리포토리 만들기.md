## 스프링 데이터 Common 커스텀 리포지토리 만들기

(쿼리 메소드로 해결이 되지 않는 경우 직접 코딩을 구현)

### 스프링 데이터 리포지토리 인터페이스에 기능 추가

JpaRepository<>를 바로 extends한 인터페이스를 사용x

새로운 interface( 아무 이름이나) 새로 만들고 거기에 정의한다.

그 후 해당 인터페이스 이름의 Impl을 붙이 클래스를 생성하고 거기서 메소드 만들기

그 후 새로운 interface를 상속한 후 거기서 @Autowired 받은 클래스 타입을 이용하여 만들기

일단 post에서 content부분은 길기 때문에 @Lob 사용

```java
// 완전 깔끔한 pojo 상태
public interface PostCustomRepository {
    List<Post> findMyPost();

}
```

```java
@Repository
@Transactional
public class PostCustomRepositoryImpl implements PostCustomRepository {
    @Autowired
    EntityManager entityManager;

    @Override
    public List<Post> findMyPost(){
        System.out.println("custom findMyPost");
        return entityManager.createQuery("SELECT p FROM Post AS p", Post.class)
                .getResultList();
    }

    // 이제 PostRepository에서 PostCustomRepository를 상속시켜서 사용하게 하면 된다.
    // 그러면 postRepository가 autowired할 때 Impl로 주입이 되어서 사용이 가능해진다.

}
```

```java
public interface PostRepository extends JpaRepository<Post, Long>, PostCustomRepository

// 이러면 Impl 설정이 온다..
// 이제 쓰고싶은 곳에서 PostRepository postRepository를 autowired해서 사용하면 됨
```

### 스프링 데이터 리포지토리 기본 기능 덮어쓰기 가능(한 Entity에 대한)

(기존 가진 메소드가 마음에 안들 때)

위와 같이 메소드를 interface에서 추가하는데 기존에 있던 것이랑 같은 이름으로 만들면된다.

```java
public interface PostCustomRepository {
    List<Post> findMyPost();
		void delete(T entity);
}
```

같은 이름이어도 사용자가 만든 것을 우선으로 해서 사용한다.

insert 하고 delete 하면 pesist 이기 때문에 애초에 insert를 하지 않고 delete도 하지 않는다.

```java
postRepository.save(post);

postRepository.findMyPost(post);

postRepository.delete(post);

// save --> insert는 실행된다 --> find에서 select에 영향을 끼치기 때문에
// delete는 실행 안된다. --> persistent 상태에서 있다가 (어차피 롤백인데 delete를 안하게 된다)

postRepository.flush()
// flush로 강제를 하면 된다. 객체 상태에선 이미 removed 상태로 변경 시켰지만 아직 얘를 데이터베이스에 싱크는 안했따
// 롤백이니까 기본적으로 @Transactional 이 붙어있는 테스트는 모두 롤백 테스트로 간주하니까 delete가 안날라갔는데 
// flush이면 싱크를 해서 delete 쿼리를 날리게 된다.
```

뒤에 Impl이라고 붙이는게 싫으면

```java
@EnableJpaRepositories(repositoryImplementationPostfix = "Default")
// 해당 Default 부분에 내가 넣고 싶은 이름을 넣어주면 된다.
```
