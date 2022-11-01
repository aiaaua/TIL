# JPA

JPA에서 사용하는 대표적인 어노테이션을 알아보자.  



##  `@Entity`

`@Entity`는 객체와 테이블을 매핑하는 어노테이션이다.  

public이나 protected 접근지정자인 기본 생성자가 필수이며  

final, enum, interface, inner 클래스에는 사용할 수 없다.  

적용할 수 있는 속성은 다음과 같다.  

- name: JPA에서 사용할 이름을 지정하고 기본값으로는 class명을 사용함



## `@Table`

`@Table`은 entity와 매핑할 테이블을 지정하고자할 때 사용한다.  

적용할 수 있는 속성은 다음과 같다.  

- name: 매핑할 테이블 이름을 설정하며 기본값으로는 entity명을 사용함
- catalog: 카탈로그 기능이 있는 데이터베이스에서 catalog를 매핑
- schema: 스키마 기능이 있는 데이터베이스에서 schema를 매핑
- uniqueConstraints(DDL): DDL 생성 시에 unique 제약조건을 만들며 2개 이상의 복합 unique 제약조건도 만들 수 있음  



## `@Id`

JPA에서 DB 기본 키를 생성하는 방식 중 직접 할당 방식이다.  

`@Id`를 적용할 수 있는 타입은 다음과 같다.  

```
- String
- java.util.Date
- java.sql.Date
- java.math.BigDecimal
- java.math.BigIntege
```





## `@GeneratedValue`

JPA에서 DB 기본 키를 생성하는 방식 중 자동 생성 방식이다.

자동 생성 방식의 전략은 다음과 같이 세 가지가 있다.  

1. IDENTITY 전략: PK 생성을 DB에 의존하여 데이터베이스에 위임(MySQL, PostgreSQL, SQL Server, DB2에 적용 가능)  
2. SEQUENCE 전략: DB에 의존하여 DB 시퀀스를 사용하여 기본키 할당(Oracle, PostgreSQL, DB2, H2에 적용 가능)  
3. TABLE 전략: DB에 의존하지 않고 키 생성 테이블을 만들어 사용(모든 DB에 적용 가능)  

여기서 TABLE 전략은 `@TableGenerator` 어노테이션을 사용하며  

적용할 수 있는 속성은 다음과 같다.  

- name: 식별자 생성기 이름을 설정가능하며 기본 값이 필수
- table: 키 생성 테이블명(default=hibernate_sequence)
- pkColumnName: 시퀀스 컬럼명(default=sequence_name)
- valueColumnName: 시퀀스 값 컬럼명(default=next_val)
- pkColumnValue: 키로 사용할 값 이름(default=Entity명)
- initialValue: 초기 값, 마지막으로 생성된 값이 기준(default=0)
- allocationSize: 시퀀스 한 번 호출에 증가하는 수, 성능 최적화에 사용(default=50)
- catalog, schema: 데이터베이스 catalog,schema 명
- uniqueConstraints(DDL): 유니크 제약 조건을 지정할 수 있음

