## 사용자가 직접 만든 Repository

내가 직접 인터페이스를 정의하고 싶은 경우 @RepositoryDefinition 을 사용해서 만든 후 이 인터페이스를 implements 해서 사용(위에 2개 메소드만 사용 가능해짐) —> 이러면 각 Entity마다 만들어야 되서 별로

공통된 메서드들을 들고가고 싶으면 @NoRepositoryBean을 사용 후 Repository를 상속해서 쓰고싶은 것들만 가져온 후(JpaRepository가서 메서드 긁어오기) 이 인터페이스를 implements 해서 사용한다.


![3](https://github.com/Croon00/JPA-study/assets/73871364/8f3a3c08-ba3c-4eab-b820-95bb410d6157)
