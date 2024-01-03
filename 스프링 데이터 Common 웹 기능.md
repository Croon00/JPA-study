## 스프링 데이터 Common 웹 기능

```java
@RestController
public class PostController {

    @Autowired
    PostRepository postRepository;

    // int 타입의 id의 pathVariable을 바로 post파라미터로 받아서 사용 가능하다.
    @GetMapping("posts/{id}")
    public String getAPost(@PathVariable("id") Post post){
        return post.getTitle();
    }

    // request 핸들러에 pageable을 바로 받을 수 있고 또는 sort를 받을 수 있다.
    // pageable 혹은 sort를 바인딩 해서 사용가능
    // hateaous --> 추구하는 의미 --> 하이퍼미디어(리소스의 정보와 같이 보내라 는 뜻)
    // hateoas 스프링 mvc일때 링크 정보를 같이 가져가는 기능
    // 페이지를 파라미터로 받아서 사용하면 페이지를 받을 수 있는데 이를 pagedResourceAssembler를 사용해서 페이지를 리소스로 변환하면
    // 페이지만 보내는 것이 아닌 링크 정보들도 같이 보낸다.
    @GetMapping("/posts")
    public PagedModel<EntityModel<Post>> getPosts(Pageable pageable, PagedResourcesAssembler<Post> assembler){
        Page<Post> all = postRepository.findAll(pageable);
        return assembler.toModel(postRepository.findAll(pageable));
    }
}

// https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web
	implementation group: 'org.springframework.boot', name: 'spring-boot-starter-web', version: '2.7.7'
// https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-hateoas
	implementation group: 'org.springframework.boot', name: 'spring-boot-starter-hateoas', version: '2.7.7'
```

## 스프링 데이터 Common 웹 기능 2

```java
@GetMapping("/posts/{id}")
	// convert 가 long 타입의 것을 post로 바로 받을 수 있게 해준다.
    public String getAPost(@PathVariable Long id) {
        Optional<Post> byId = postRepository.findById(id);
        Post post = byId.get();
        return post.getTitle();
    }
```

```java
@GetMapping("/posts/{id}")
    public String getAPost(@PathVariable(“id”) Post post) {
        return post.getTitle();
    }
```

[Converter (Spring Framework 6.0.4 API)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/convert/converter/Converter.html)

Formatter도 들어 본 것 같은데… —> String으로 변환 해주는 것

```java
Post post = new Post();
        post.setTitle("jpa");
        postRepository.save(post);

        // 밑에 것이 없으면 insert가 안 날라간다 -->
        // Test기 때문에 기본 롤백이기 때문에 insert 해봤자 어차피 롤백할거자나? 하고 안 실행해버림
        // 그럼으로 밑에 select 하는 것을 실행 시켜야 insert를 실행한다.
        List<Post> all = postRepository.findAll();
        assertThat(all.size()).isEqualTo(1);
```

```java
Post post = new Post();
        post.setId(1l);
        post.setTitle("jpa");
        postRepository.save(post);

        // id가 똑같아서 두번째는 사실상 update 쿼리가 발상하게 된다.
        // 새로운 객체가 아닐 경우 --> entityManager에 merge가 실행된다.(persist가 아닌) persist --> transient에서 persistent로 바꿈
        // transient는 새로 만들어진 객체라서 hibernate나 database 모두 모른다.
        // detached 상태는 한 번이라도 database에 persistent가 되었던 객체 --> 즉 이 객체에 매핑이 되는 레코드가 테이블에 있는 경우(Id)가 매핑되는 것이 테이블에 레코드에 존재하는 경우
        // 왜 persistent를 사용하지? 그냥 merge 사용하면되는가?
        Post postUpdate = new Post();
        postUpdate.setId(1l);
        postUpdate.setTitle("hibernate");
        postRepository.save(postUpdate);
```

Transient인지 Detached 인지 어떻게 판단 하는가?

- 엔티티의 @Id 프로퍼티를 찾는다. 해당 프로퍼티가 null이면 Transient 상태로 판단하고 id가 null이 아니면 Detached 상태로 판단한다.
- 엔티티가 Persistable 인터페이스를 구현하고 있다면 isNew() 메소드에 위임한다.
- JpaRepositoryFactory를 상속받는 클래스를 만들고 getEntityInfomration()을 오버라이딩해서 자신이 원하는 판단 로직을 구현할 수도 있습니다.

```java
// 단순히 저장이 일어나는 것이 아닌다

        Post post = new Post();
        post.setTitle("jpa");
        postRepository.save(post); //persist 호출
        
        Post postUpdate = new Post();
        postUpdate.setId(post.getId());
        postUpdate.setTitle("hibernate");
        postRepository.save(postUpdate); // merge
```

```java
Post post = new Post();
        post.setTitle("jpa");
        // 항상 리턴되는 인스턴스가 있다 --> save를 사용했을 때 리턴되는 객체 -- 항상 영속화되는 객체를 리턴해준다.
        Post savedPost = postRepository.save(post); //persist 호출
        // persist에 넘겨준 이 인스턴스가 영속화가 된다.

        // persistentContext가 이 인스턴스를 캐싱(가지고 있다)
        assertThat(entityManager.contains(post)).isTrue();
        assertThat(entityManager.contains(savedPost)).isTrue();
        assertThat(savedPost == post);
        
        Post postUpdate = new Post();
        postUpdate.setId(post.getId());
        postUpdate.setTitle("hibernate");
        Post updatePost = postRepository.save(postUpdate); // merge
        // jpa대신에 hibernate로 값을 업데이트 하게 된다.
        // 이 인스턴스는 merge를 사용했기 때문에 전달받은 파라미터에 복사본을 만들어서 데이터베이스에 영속화한다.
        // 이 복사본을 persistent상태로 만들지 않는다.

        
        // 이 둘의 객체는 같지 않다.
        //얘는 persitent 상태로 가고
        assertThat(entityManager.contains(updatePost)).isTrue();
        // 애는 persistent 상태로 가지 않는다.
        assertThat(entityManager.contains(postUpdate)).isFalse();
        assertThat(updatePost == postUpdate);

				// 얘는 persitent 상태의 객체가 아니라서 상태변화를 감지를 못한다 --> 그냥 hibernate
        postUpdate.setTitle("kkk");
        // persitent상태의 객체를 사용한다면 결과는 달라진다 --> 객체 상태의 변화를 추적한다. 이 객체 상태의 변화가 필요한 경우에 반영이된다.
        // 밑에 findall 할때 실행된다
        updatePost.setTitle("kkk1");
```

—> 무조건 return 받은 instance를 사용해라
