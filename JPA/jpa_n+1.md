## N + 1 문제
- N+1 문제란, ORM(Object-Relational Mapping)을 사용하는 애플리케이션에서 관계형 데이터베이스와의 데이터 조회 작업에서 발생할 수 있는 성능 문제  

- N+1 문제는 한 번의 초기 쿼리 실행으로 가져온 데이터를 사용하는 도중 추가로 N번의 쿼리를 실행해야 하는 상황을 말한다.

- 이로 인해 데이터베이스와의 불필요한 네트워크 통신이 발생하며, 성능 저하와 불필요한 데이터베이스 부하를 초래할 수 있습니다.

<br>

### 예시

#### 관계: Author (1) - Book (N)

```java
@Entity
public class Author {
    // ...
    
    @OneToMany(mappedBy = "author", fetch = FetchType.EAGER)
    private List<Book> books;
    
    // ...
}

@Entity
public class Book {
    // ...
    
    @ManyToOne(fetch = FetchType.EAGER)
    private Author author;
    
    // ...
}
```

#### 데이터 생성 및 테스트
```java
// 데이터 생성
Author author = new Author("John Smith");
Book book1 = new Book("Book 1", author);
Book book2 = new Book("Book 2", author);
List<Book> books = Arrays.asList(book1, book2);

// 저장
authorRepository.save(author);
bookRepository.saveAll(books);

// 조회
Author retrievedAuthor = authorRepository.findById(author.getId()).orElse(null);

// 결과 출력
System.out.println(retrievedAuthor.getName()); // John Smith
System.out.println(retrievedAuthor.getBooks().size()); // 2
```

#### SQL log 확인
```
// 초기 쿼리 (Author 조회)
SELECT * FROM author WHERE id = 1;

// 추가 쿼리 (Books 조회)
SELECT * FROM book WHERE author_id = 1;
SELECT * FROM book WHERE author_id = 1;
```
- id가 1인 작가를 찾는 쿼리문을 호출했는데, 작가가 보유한 책 만큼 책을 호출하는 쿼리문이 출력된 걸 확인할 수 있다. (N + 1)

- 즉, 작가를 조회하는 쿼리를 한 번 실행하고, 작가가 보유한 책을 조회하는 쿼리를 N번 실행하는 상황이 발생하였다.

#### FetchType을 LAZY로 변경
```java
public class Author {
    // ...
    
    @OneToMany(mappedBy = "author", fetch = FetchType.LAZY)
    private List<Book> books;
    
    // ...
}
```

#### SQL log 확인
```
// 초기 쿼리 (Author 조회)
SELECT * FROM author WHERE id = 1;

// 추가 쿼리 (Books 조회)
SELECT * FROM book WHERE author_id = 1;
SELECT * FROM book WHERE author_id = 1;
```
- FetchType을 LAZY로 변경하였지만, N + 1 문제가 여전히 발생하고 있다.
- N + 1 문제는 FetchType.EAGER를 사용하더라도 발생할 수 있으며, FetchType.LAZY를 사용하더라도 발생할 수 있다. 
- 이유는 JPA의 Fetch 전략이 연관 엔티티를 로딩하는 시점에만 영향을 미치기 때문이다.

### FetchType.EAGER vs FetchType.LAZY
- FetchType.EAGER의 경우 순서는 다음과 같다
    1. JPQL에서 만든 SQL을 통해 데이터를 조회
    2. 이후 JPA에서 Fetch 전략을 가지고 해당 데이터의 연관 관계인 하위 엔티티들을 추가 조회
    3. 2번 과정으로 N + 1 문제 발생

- FetchType.LAZY의 경우 순서는 다음과 같다
    1. JPQL에서 만든 SQL을 통해 데이터를 조회
    2. JPA에서 Fetch 전략을 가지지만, 지연 로딩이기 때문에 추가 조회는 하지 않음
    3. 하지만, 하위 엔티티를 가지고 작업하게 되면 추가 조회가 발생하기 때문에 결국 N + 1 문제 발생

### N + 1 발생 원인
- JpaRepository에 정의한 인터페이스 메서드를 실행하면 JPA는 메서드 이름을 분석해서 JPQL을 생성하여 실행하게 된다. 
- JPQL은 SQL을 추상화한 객체지향 쿼리 언어로서 특정 DB SQL에 종속되지 않고 엔티티 객체와 필드 이름을 가지고 쿼리를 한다.   
- 그렇기 때문에 JPQL은 findAll()이란 메소드를 수행하였을 때 해당 엔티티를 조회하는 `select * from Author` 쿼리만 실행하게 되는것이다.   
- JPQL 입장에서는 연관관계 데이터를 무시하고 해당 엔티티 기준으로 쿼리를 조회하기 때문에 연관된 엔티티 데이터가 필요한 경우, FetchType으로 지정한 전략에 따라 추가 쿼리를 실행하게 된다.

<br>

## 해결 방법
### Fetch join

```java
@Entity
public class Author {
    // ...

    @OneToMany(fetch = FetchType.LAZY)
    @JoinColumn(name = "author_id")
    private List<Book> books;

    // ...
}

@Repository
public interface AuthorRepository extends JpaRepository<Author, Long> {
    
    // Fetch join을 적용한 쿼리
    @Query("SELECT a FROM Author a JOIN FETCH a.books WHERE a.id = :id")
    Optional<Author> findByIdWithBooks(Long id);
}

Optional<Author> retrievedAuthor = authorRepository.findByIdWithBooks(author.getId());

// 결과 출력
System.out.println(retrievedAuthor.get().getName()); // John Smith
System.out.println(retrievedAuthor.get().getBooks().size()); // 2
```

- Author 엔티티에서 books 필드에 Fetch Join을 적용하여 작가와 연관된 책을 한 번에 가져옵니다.
- AuthorRepository의 findByIdWithBooks() 메서드는 Fetch Join이 적용된 쿼리를 실행하여 작가와 연관된 책의 정보를 함께 가져옵니다.

**단점**
- Fetch join은 필요한 모든 연관 엔티티를 한 번에 가져오기 때문에 데이터의 양이 많을 경우 성능 저하가 발생할 수 있습니다. 
- Fetch join은 JPA 구현체에 의존하기 때문에 이식성이 떨어질 수 있습니다.

<br>

### EntityGraph
```java
// EntityGraph를 사용한 코드
@Entity
public class Author {
    // ...

    @OneToMany(mappedBy = "author", fetch = FetchType.LAZY)
    private List<Book> books;

    // ...
}

@Repository
public interface AuthorRepository extends JpaRepository<Author, Long> {
    
    // EntityGraph를 적용한 쿼리
    @EntityGraph(attributePaths = "books")
    Optional<Author> findById(Long id);

}

Optional<Author> retrievedAuthor = authorRepository.findById(author.getId());

// 결과 출력
System.out.println(retrievedAuthor.getName()); // John Smith
System.out.println(retrievedAuthor.getBooks().size()); // 2
```

- Author 엔티티에서 books 필드에 EntityGraph를 적용하여 작가와 연관된 책을 한 번에 가져오는 방식입니다. 
- EntityGraph를 사용하여 작가 엔티티 조회 시 즉시 필요한 연관 엔티티를 로드할 수 있습니다.

**단점**
- EntityGraph를 사용하면 JPA의 표준을 벗어나는 방식으로 데이터를 로드할 수 있습니다. 
- EntityGraph를 사용하면 복잡한 쿼리 작성이 필요하고, 유지보수가 어려울 수 있습니다.

<br>

### BatchSize
- JPA 및 Hibernate에서 다수의 연관된 엔티티를 가져올 때 성능을 최적화하기 위해 사용되는 어노테이션
- 한 번에 지정된 개수의 엔티티를 가져와 N+1 문제를 줄이는 데 도움이된다.

```java
// BatchSize를 사용한 코드
@Entity
public class Author {
    // ...

    @OneToMany(mappedBy = "author", fetch = FetchType.LAZY)
    @BatchSize(size = 10)
    private List<Book> books;

    // ...
}

// BatchSize를 적용한 조회
Author retrievedAuthor = authorRepository.findById(author.getId()).orElse(null);
Hibernate.initialize(retrievedAuthor.getBooks());

// 결과 출력
System.out.println(retrievedAuthor.getName()); // John Smith
System.out.println(retrievedAuthor.getBooks().size()); // 2
```

- Author 엔티티에서 books 필드에 BatchSize를 적용하여 일괄 로딩을 수행하는 방식입니다. 
- @BatchSize 어노테이션을 사용하여 한 번의 쿼리로 지정된 개수의 연관 엔티티를 로드합니다.

**단점**
- BatchSize는 일괄 로딩을 수행하지만, 연관 엔티티가 많은 경우 여전히 N+1 문제가 발생할 수 있습니다. 
- 성능을 향상시키기 위해 적절한 BatchSize 값을 설정해야 합니다.

### QueryBuilder
```java
// QueryBuilder를 사용한 코드
CriteriaBuilder cb = entityManager.getCriteriaBuilder();
CriteriaQuery<Author> query = cb.createQuery(Author.class);
Root<Author> root = query.from(Author.class);
root.fetch("books", JoinType.LEFT);
query.select(root).where(cb.equal(root.get("id"), author.getId()));

Author retrievedAuthor = entityManager.createQuery(query).getSingleResult();

// 결과 출력
System.out.println(retrievedAuthor.getName()); // John Smith
System.out.println(retrievedAuthor.getBooks().size()); // 2
```

- Criteria API를 사용하여 작가와 연관된 책을 함께 조회하는 방식입니다. 
- QueryBuilder를 통해 작가와 연관된 책의 정보를 로드하고, 한 번의 쿼리로 결과를 가져옵니다.

**단점**
- QueryBuilder를 사용하면 복잡한 쿼리 작성이 필요하고, JPA의 객체 지향적인 특성을 잃을 수 있습니다.
- 유지보수가 어려울 수 있습니다.