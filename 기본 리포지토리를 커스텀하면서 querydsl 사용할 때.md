## 기본 리포지토리를 커스텀하면서 querydsl 사용할 때

복잡한 쿼리를 만들때는 (Extension(interface)—> querydsl로 만들 메소드만 을 만들어서 이 내에서 (Impl에서—> 구현체(프래그먼츠)))

QuerydslRepositorySupport를 상속 받아서

JPQLQuery 타입을 통해서 쿼리를 생성할 수 있다.

```java
JPQLQuery<Study> query = from(study).where(study.published.isTrue()
													.and(study.title.containsIgnoreCase(keyword))
													.or(study.tags.any().title.containsIgnoreCase(keyword)) 
.....

```

내가 모든 리포지토리에 추가하고 싶을때

```java
//중간에 들어가는 리포지토리는 @NoRepository해주어야 한다.
@NoRepositoryBean
public interface CustomRepository<T, ID extends Serializable> extends JpaRepository<T, ID> {

// T타입의 entity가 Jpa 혹은 하이버네이트에 들어있는지 확인해주는 영속성 컨텍스트에 추가 되있는지 확인해주는 
// contains라는 메소드를 몯느 리포지토리에 추가하고싶다.
    boolean contains(T entity);
}
```

```java
package com.example.querydsl.account;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.EntityManager;
import org.springframework.data.jpa.repository.support.JpaEntityInformation;
import org.springframework.data.jpa.repository.support.SimpleJpaRepository;

public class CustomRepositoryImpl<T,ID extends Serializable> extends SimpleJpaRepository<T, ID> implements CustomRepository<T ,ID> {

    private EntityManager entityManager;
    // 나머지 기능들 --> JpaRepository들을 SimpleRepository가 가지고 있다 --> 구현 할 필요 x, 생성자를 만들어줘야 한다.(Bean을 주입받아서 전달)
    public CustomRepositoryImpl(
            JpaEntityInformation<T, ?> entityInformation,
            EntityManager entityManager) {
        super(entityInformation, entityManager);
    }

    // 추가하고싶은 기능을 가지고 있는 것
    @Override
    public boolean contains(T entity) {
        return entityManager.contains(entity);
    }
}
```

```java
package com.example.querydsl;

import com.example.querydsl.account.CustomRepositoryImpl;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

@SpringBootApplication
// 구현체를 spring data jpa는 모름으로 이를 알려주어야 한다.
@EnableJpaRepositories(basePackageClasses = CustomRepositoryImpl.class)
public class QuerydslApplication {

	public static void main(String[] args) {
		SpringApplication.run(QuerydslApplication.class, args);
	}

}
```

```java
public interface AccountRepository extends JpaRepository<Account, Long> {

}
// 가 아닌
public interface AccountRepository extends CustomRepository<Account, Long>, QuerydslPredicateExecutor<Account> {

}

```
