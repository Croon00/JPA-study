## cascade

1. 삭제될때 전파를 하라고 cascadeType.PERSIST
2. Transient에서 PERSISTENT로 넘어갈 때 참조하고 있던 객체들도 PERSISTENT로 넘어간다. 그리고 같이 저장이된다.
3. {}를 통해서 여러가지를 설정할 수도 있다.
4. cascade 없이 [post.save](http://post.save) 했을 때 정말 post만 저장된다. comment 테이블에 까지 같이 저장하려면 casecade필요
5. child에 있던 객체들도 같이 Persistent 상태가 되어서 저장이 됨으로 실행된다.
6. cascadeType.REMOVE 상태 면 같이 삭제됨

```java
// child한 속성 외래키에 이렇게 적용을 하면 
// post만 save해도 자동으로 comment 테이블에도 insert가 일어나게 된다.
@OneToMany(mappedBy = "post", cascade = {CascadeType.PERSIST, CascadeType.REMOVE})
    private Set<Comment> comments = new HashSet<>();
```
