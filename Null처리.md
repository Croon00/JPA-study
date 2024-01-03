## Null 처리

NPE (null point Exception)을 나지 않기 위해서 하는 것

Java 8 부터 제공해준 Optional을 이용하면 길게 코딩해서 null을 처리하는 것이 아닌 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE가 발생하지 않도록 한다.

[[Java] Optional이란? Optional 개념 및 사용법 - (1/2)](https://mangkyu.tistory.com/70)

1. Optional<Post> findById(Long id);

1. 스프링 프레임워크 5.0부터 지원하는 Null 애노테이션을 지원한다.
2. @NonNullApi , @NonNull @Nullable

@NonNull  —> 널을 지원하지 않는 

@Nullable —> 널이 와도 가능한
