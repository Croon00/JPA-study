## @Embeddable

1. 위치정보를 다루는 것이면 충분히 Entity가 될 수 있지만 Composite Value 타입 매핑!
2. Account에 속한 데이터 중에 하나로 취급한다. --> Entity로 보긴 어렵고 종속적인 타입 --> Value 타입 @Embeddable 사용
3. @Column 생략됨
4. Value 타입 종류 --> 기본 타입 (String, Date, Bollean)
5. Collection Value 타입 --> 기본 타입 콜렉션 , 컴포짓 타입의 콜렉션

// Address 2개 사용 할 때

// street을 home_street으로 사용하기

// Address에 있는 속성 값을 overide 해서 이름을 변경해서 사용 하겠다.

```java
@Embedded
    @AttributeOverride({
       @AttributeOverride(name = "street", column = @Column(name = "home_street")
    })
private Address homeAddress;
```
