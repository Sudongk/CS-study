# JPA 와 Hibernate

## JPA (Java Persistence API)
- 자바 진영의 ORM 기술 표준
- ORM(Object-Relational Mapping) 기술을 사용하여 객체와 관계형 데이터베이스를 매핑하는 표준 기술
- JPA를 사용하면 개발자는 객체 중심으로 개발을 할 수 있고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성하여 실행한다.

> 자바의 ORM 객체 관계 매핑 기술의 표준 인터페이스의 모음으로 따라서 실제로 사용하기 위해서는 JPA를 구현한 Hibernate, EclipseLink, DataNucleus 등의 ORM 프레임워크를 사용해야 한다.

> **ORM** : 객체와 관계형 데이터베이스의 데이터를 자동 매핑해 주는 기술로 객체는 객체대로, 관계형 데이터베이스는 관계형 데이터베이스대로 설계하고, ORM 프레임워크가 중간에서 매핑해준다.

> **Hibernate** : JPA의 구현체 중 하나

## 장점
- 개발자는 객체지향적 프로그래밍을 하고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성하여 실행해줘서 더는 SQL에 종속적인 개발을 하지 않아도 된다.
- JPA는 인터페이스로 추상화된 데이터베이스 접근 기술이기 떄문에 데이터베이스 변경시 
- 기본적이고, 반복적인 CRUD(Create, Read, Update, Delete) 쿼리를 자동으로 생성해주기 때문에 개발자의 생산성을 높일 수 있다.
- JPA는 성능 최적화 기능을 제공한다.
    - 1차 캐시와 동일성(identity) 보장 : 같은 트랜잭션 안에서는 DB에서 가져온 엔티티를 캐시에 저장하여 다음에 같은 엔티티를 요청할 캐시에서 가져오기 때문에 DB와의 통신을 줄일 수 있다.
    - 트랜잭션 쓰기 지연 : 트랜잭션을 커밋할 때까지 쓰기 지연 SQL 저장소에 쿼리를 모아두었다가 커밋할 때 한번에 쿼리를 실행하기 때문에 DB와의 통신을 줄일 수 있다.
    - 지연 로딩 : 연관된 엔티티를 실제 사용할 때 로딩하기 때문에 불필요한 조인을 피할 수 있다.
    - 즉시 로딩 : 연관된 엔티티를 한번에 로딩

## 단점
- 성능 이슈가 발생할 수 있다.
    - 복잡한 쿼리를 작성할 때는 JPA의 기능만으로는 한계가 있을 수 있다.
    - 이런 경우에는 직접 SQL을 작성해야 할 수도 있다. (Native Query)
- 프로젝트의 규모가 커질수록 복잡해질 수 있다.

## 매핑 어노테이션
- `@Entity` : JPA가 관리할 객체를 테이블과 매핑할 때 사용
    > 사용시 주의사항 
    > - 접근 제어자가 'public' 또는 'protected' 인 기본 생성자가 반드시 있어야 한다.
    > - final 클래스, enum, interface, inner 클래스에는 사용할 수 없다.
- `@Table` : 엔티티 클래스와 매핑할 테이블을 지정
    - `name` : 매핑할 테이블 이름
    - `indexes` : 데이터베이스 인덱스를 생성할 때 사용
- `@Id` : 엔티티 클래스의 필드를 테이블의 기본 키에 매핑할 때 사용
- `@GeneratedValue` : 기본 키의 값을 자동으로 생성할 때 사용
    - `GenerationType.IDENTITY` : 기본 키 생성을 데이터베이스에 위임
    - `GenerationType.SEQUENCE` : 데이터베이스 시퀀스를 사용하여 기본 키 생성
    - `GenerationType.TABLE` : 키 생성 전용 테이블을 사용하여 기본 키 생성
    - `GenerationType.AUTO` : 데이터베이스 벤더에 따라 자동으로 선택
- `@Column` : 필드와 테이블의 컬럼을 매핑할 때 사용
    - `name` : 필드와 매핑할 테이블의 컬럼 이름
    - `nullable` : null 값의 허용 여부
    - `unique` : 유니크 제약 조건
    - `length` : 문자열 길이 제약 조건
    - `precision` : 소수점 자릿수
    - `scale` : 소수점 이하 자릿수
- `@Enumerated` : enum 타입을 매핑할 때 사용
- `@AttributeOverride` : 속성 재정의
- `@Lob` : BLOB, CLOB 타입을 매핑할 때 사용
- `@Transient` : 테이블의 컬럼과 매핑하지 않을 때 사용
- `@OneToOne` : 일대일 관계 매핑
- `@OneToMany` : 일대다 관계 매핑
    - mappedBy : 연관 관계의 주인을 지정
    - cascade : 영속성 전이 기능
        - `CascadeType.ALL` : 모두 적용
        - `CascadeType.PERSIST` : 영속
        - `CascadeType.REMOVE` : 삭제
        - `CascadeType.MERGE` : 병합
        - `CascadeType.DETACH` : 분리
        - `CascadeType.REFRESH` : 새로고침
    - orphanRemoval : 고아 객체 제거 기능
- `@ManyToOne` : 다대일 관계 매핑
    - `fetch` : 연관 관계를 가져올 때 사용
        - `FetchType.EAGER` : 즉시 로딩
        - `FetchType.LAZY` : 지연 로딩
- `@JoinColumn` : 조인 컬럼을 매핑할 때 사용
    - `name` : 매핑할 조인 컬럼 이름
    - `foreignKey` : 외래 키 제약 조건을 지정
    - `referencedColumnName` : 참조할 대상 테이블의 컬럼명  
    - `columnDefinition` : DDL 생성 시 컬럼 정보를 정의

## 

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
