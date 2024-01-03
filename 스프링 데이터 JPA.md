## 스프링 데이터 JPA

### JpaRepository<Entity, id> 인터페이스

1. 매직 인터페이스
2. @Repository 가 없어도 등록해준다

### @EnableJpaRepositories

여기부터 시작 (원래는 이것을 붙여주어야 한다.)

### @JpaRepositoriesRegister.class

이것으로 시자한다. 이것이 import 되어있다. (BeanDefinition을 정의할 수 있는) 이는 Spring Frame Work이다..

![1.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0527f688-1dc8-4be7-b262-b0c33c23f3ae/1.png)

## Dao의 기능을 해준다!

JpaRepository의 위에 PagingAndSorttingRepository가 존재하게 되고 그 위에 CrudRepository 그 위 에 Repository가 존재한다.

중간단계의 Repository에는 @NoRepositoryBean 가 붙어있다. 이것은 진짜 리포지토리가 아니다!

제일 위에 Repsitory를 확인하면 메서드들을 확인 가능
