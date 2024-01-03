## Projection

일부분에 대한 부분 select 할 때 (*로 가져오는게 아닌)

인터페이스 기반과 클래스 기반 2개

오픈프로젝션, 클로즈 프로젝션 2개

```java
public interface CommentSummary {
		// 밑에 3개는 클로즈
    String getComment();

    int getUp();

    int getDown();

    // 코멘트를 전부 가져올 수 밖에 없다 --> 오픈 프로젝션
		// 성능 최적화 x 
		// 마치 우리가 sql에서 alis 같은 효과는 낼 수 있다.
    @Value("#{target.up + ' ' + target.down}")
    String getVotes();

		// 이 방법을 사용하면 커스텀한 구현체를 사용해서 메소드를 추가할 수도 있고 
		// 그러면서도 우리가 사용할 필드를 한정적하게 된다.
    // 쿼리는 최적하 되고, 우리가 커스텀하게 계산해낼 수도 있다.
    // 이 방법이 좋다
		// 클로즈 하면서 가능하게
		default String getVotes() {
			return getUp() + " " + getDown();
		}

}
```

```java
// 어떠한 포스트 안에 있는 코멘트 모두 조회
//    List<Comment> findByPost_id(Long id);

    // 클로즈드 프로젝션 --> 필요한 것만 가져오기
    List<CommentSummary> findByPost_id(Long id);
```
