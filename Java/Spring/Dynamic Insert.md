# Dynamic Insert

`@DynamicInsert`는 JPA를 사용하여 DB에 insert를 할 때, null인 field값이 제외되게 하기 위해 사용한다.   

`@Entity` 객체에 `@DynamicInsert` 어노테이션을 붙여 사용할 수 있다.  

## Example

예시 코드와 함께 쿼리를 살펴보면 다음과 같다.  

```java
@Entity
@Getter
@Table(name = "MEDIA")
@NoArgsConstructor
public class Media {

    @Id
    @GeneratedValue(strategy = IDENTITY)
    @Column(name = "idx")
    private Long idx;

    @Column(name = "name")
    private String name;

    @Column(name = "url")
    private String url;

    @Column(name = "like_count")
    @ColumnDefault("0") //default 0
    private Integer likeCount;

    @Builder
    public Media(String name, String url, Integer likeCount) {
        this.name = name;
        this.url = url;
        this.likeCount = likeCount;
    }
}
```

`likeCount`에 0이라는 default값을 주더라도

```java
@Test
public void insert_test() {
    Media media = Media.builder()
            .name("mediaName")
            .url("http://zum.com")
            .build();

    Media resultMedia = mediaRepository.save(media);
}
```

`likeCount`에 값을 주지 않으면 DB insert가 실패하는 것을 확인할 수 있다.  

이 때 실제 수행되는 쿼리를 살펴보면

```sql
Hibernate: 
    insert 
    into
        media
        (idx, like_count, name, url) 
    values
        (null, ?, ?, ?)
```

이와 같으며 `like_count`에 null이 들어가게 되어 insert가 실패하는 것이다.  

`Media` 클래스에 `@DynamicInsert`어노테이션을 붙여주게 되면 쿼리가 아래와 같이 바뀐다.  

```sql
Hibernate: 
    insert 
    into
        media
        (idx, name, url) 
    values
        (null, ?, ?)
```

`like_count`값을 넣어주지 않아 null값이기 때문에 쿼리에서 이 부분이 제외되는 것이고,  

이렇게 할 경우 정상적으로 DB에 insert가 되는 것을 확인할 수 있다.  

## Reference
- https://dotoridev.tistory.com/6
- https://velog.io/@titu/Spring-JPA-JPA%EC%97%90%EC%84%9C-insert%ED%95%98%EB%8A%94-%EA%B2%BD%EC%9A%B0-DB%EC%9D%98-default%EA%B0%92%EC%9D%B4-%EC%A0%81%EC%9A%A9%EB%90%98%EC%A7%80-%EC%95%8A%EC%9D%84-%EB%95%8C-DynamicInsert
