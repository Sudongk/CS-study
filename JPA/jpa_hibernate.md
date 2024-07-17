# JPA 와 Hibernate

## JPA (Java Persistence API)
- 자바 진영의 ORM 기술 표준
- ORM(Object-Relational Mapping) 기술을 사용하여 객체와 관계형 데이터베이스를 매핑하는 표준 기술
- JPA를 사용하면 개발자는 객체 중심으로 개발을 할 수 있고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성하여 실행한다.

> **Hibernate** : JPA의 구현체 중 하나

## 장점
- 객체 중심으로 개발을 할 수 있어 객체지향적인 코드를 작성할 수 있다.
- 데이터베이스에 대한 종속성이 줄어들어 유지보수가 용이하다.
- 기본적이고, 반복적인 CRUD(Create, Read, Update, Delete) 쿼리를 자동으로 생성해주기 때문에 개발자의 생산성을 높일 수 있다.
- 객체와 테이블간 매핑을 개발자가 직접 하지 않아도 되기 때문에 객체지향적인 설계를 할 수 있다.

## 단점
- 성능 이슈가 발생할 수 있다.
    - 복잡한 쿼리를 작성할 때는 JPA의 기능만으로는 한계가 있을 수 있다.
    - 이런 경우에는 직접 SQL을 작성해야 할 수도 있다. (Native Query)
- 프로젝트의 규모가 커질수록 복잡해질 수 있다.

## 사용 예시
```java
@Entity
public class Member {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private int age;
    
    // Getter, Setter

}
```

```java
public class MemberRepository {

    @PersistenceContext
    private EntityManager em;
    
    public void save(Member member) {
        em.persist(member);
    }
    
    public Member find(Long id) {
        return em.find(Member.class, id);
    }
    
    public void update(Long id, String name) {
        Member member = em.find(Member.class, id);
        member.setName(name);
    }
    
    public void delete(Long id) {
        Member member = em.find(Member.class, id);
        em.remove(member);
    }

}
```

### 테스트 코드
```java
@SpringBootTest
@Transactional
class MemberServiceTest {

    @Autowired
    MemberRepository memberRepository;

    @BeforeEach
    void setUp() {
        memberRepository.deleteAll();
    }

    @Test
    void save() {
        Member member = new Member();
        member.setName("memberA");

        Long saveId = memberService.save(member);

        Member findMember = memberService.findOne(saveId).get();
        assertThat(findMember).isEqualTo(member);
    }

    @Test
    void find() {
        Member member1 = new Member();
        member1.setName("member1");
        memberService.save(member1);

        Member member2 = new Member();
        member2.setName("member2");
        memberService.save(member2);

        Member findMember1 = memberService.findOne(member1.getId()).get();
        Member findMember2 = memberService.findOne(member2.getId()).get();

        assertThat(findMember1).isEqualTo(member1);
        assertThat(findMember2).isEqualTo(member2);
    }

    @Test
    void update() {
        Member member = new Member();
        member.setName("memberA");
        Long saveId = memberService.save(member);

        String updateName = "memberB";
        memberService.update(saveId, updateName);

        Member findMember = memberService.findOne(saveId).get();
        assertThat(findMember.getName()).isEqualTo(updateName);
    }

    @Test
    void delete() {
        Member member = new Member();
        member.setName("memberA");
        Long saveId = memberService.save(member);

        memberService.delete(saveId);

        Optional<Member> findMember = memberService.findOne(saveId);
        assertThat(findMember).isEmpty();
    }

}
```
