# delete와 deleteById

Spring Data JPA에서 `delete`와 `deleteById`는 CrudRepository interface에 정의되어있는 메서드로  

DB에 delete 쿼리를 날릴 수 있는 메서드이다.  

`JpaRepository<T, ID> interface`를 상속받은 Repository interface를 생성해준 뒤 사용이 가능하다.  

```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```

각 메서드는 Service에서 아래와 같이 구현하여 사용할 수 있다.  

```java
@Service
public class UserService {
    
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    // delete 메서드 사용
    public  void delete(User user) {
        userRepository.delete(user);
    }
    
    // deleteById 메서드 사용
    public void deleteById(Long userId) {
        userRepository.deleteById(userId);
    }
}
```

위의 `delete`와 `deleteById`메서드를 사용한 결과는 동일하다.  

로직에서의 차이를 살펴보면 `deleteById`는 아래와 같이 구현되어있다.  

- id값으로 findById를 사용하여 delete할 데이터 조회
- 단, id가 null일 경우 _EmptyResultDataAccessException_ 발생
- delete 호출하여 데이터 삭제

따라서 `deleteById`는 `delete`에 `findById`를 결합해두고, 또 id 값이 null일 경우 exception을 발생하게 한다는 것에서 `delete` 메서드와 차이가 있다.  

두 메서드의 차이점으로 미루어보았을 때,  

- 메서드를 하나만 사용하여 조회 및 삭제를 원함, null체크 -> `deleteById`
- `findById`의 예외처리를 직접 커스텀하길 원함 -> `delete`

상황에 맞게 필요한 것을 사용하면 될 것 같다.  



##  Reference

- https://hwanchang.tistory.com/7