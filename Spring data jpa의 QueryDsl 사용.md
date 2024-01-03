## Spring data jpa의 QueryDsl 사용

여러 쿼리 메소드는 대부분 두 가지 중 하나

1. findOne(Predicate) : 이런저런 조건으로 무언가 하나를 찾는다 —> Optional을 리턴
2. findAll(Predicate) : 이런 저런 조건으로 무언가 여러개를 찾는다. —> List, Page

![7](https://github.com/Croon00/JPA-study/assets/73871364/705a941f-cff4-42a7-849d-83950b674a79)

```java
Predicate predicate = QAccount.account
											.firstName.containsIgnoreCase("kim")
											.and(account.lastName.startWith("back"));
Optional<Account> one = accountRepository.findOne(predicate);

```

[Querydsl - Unified Queries for Java](http://querydsl.com/)

[Spring Data JPA - Reference Documentation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.manual-wiring)

querydsl —> 쿼리 조건문들을 만들때 타입 safe 하다 자바 코드로 만들 수 있다.

기존 @Query를 사용해서 query문을 사용하는 에서는 한계가 존재한다.

@Query와 Jpql 차이

[[JPA] @Query, 직접 쿼리 작성](https://jforj.tistory.com/90)

JPQL로 하는 법에서도 한계가 존재

[JPQL vs QueryDSL 간단 비교](https://devkingdom.tistory.com/242)

조건문들끼리 조합할 수도 있다. 별도의 클래스에 모아서 사용할 수 있다.

Querydsl —> 

[[JPA] QueryDSL](https://jeonyoungho.github.io/posts/QueryDSL/)

BookRepository extends JpaRepository<Book, Long>에다가 QuerydslPredicateExecutor를 추가하면 

predicate을 받는 여러가지 메소드를 사용 가능하다. (querydsl 플러그인들을 설정해야 한다)

```java
Optional<Book> ring = bookRepository.findOne(QBook.book.title.contains("ring"));
// 인터페이스를 상속 해서 
```
