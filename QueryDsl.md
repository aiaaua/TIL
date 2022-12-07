# QueryDsl

`QueryDsl`은 Java의 프레임워크 중 하나로 HQL(Hibernate Query Language) 쿼리를 타입에 안전하게 생성 및 관리할 수 있도록 해준다.  

복잡한 프로젝트 환경에서 JPA만으로 해결하기 어려운 문제들을 해결할 수 있다.  

JPQL보다 더 직관적이며 컴파일 단계에서 오류를 해결할 수 있다는 장점이 있다.  



## Example

JPA로만 해결하기 어려운 것은 네이티브 쿼리를 이용할 수도 있는데,  

네이티브 쿼리를 사용하여 다음과 같은 쿼리를 작성했다고 하자.  

```java
@Query(value = "SELECT id, title, user_id FROM article WHERE user_id IN (SELECT id FROM user WHERE level > :level)", nativeQuery = true)
List<Article> findByLevel(String level);
```

이를 `QueryDsl`로 변경하면 다음과 같이 작성할 수 있다.

```java
public List<Article> findByUserLevel(String level) {
    QArticle article = QArticle.article;
    QUser user = QUser.user;

    return queryFactory.selectFrom(article)
        .where(
            article.userId.in(
                JPAExpressions
                    .select(user.id)
                    .from(user)
                    .where(user.level.gt(level))
            )
        )
        .fetch();
}
```

전체적인 길이는 더 길어지지만 훨씬 직관적이다.  



##  Reference

- https://querydsl.com/static/querydsl/4.4.0/reference/html_single/#intro

- https://madplay.github.io/post/introduction-to-querydsl

  