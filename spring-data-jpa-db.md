## 🗃️ Spring Data JPA & Database Integration (Detailed)

### Repository Interfaces
Spring Data JPA reduces boilerplate code by providing interfaces for data access layers. You just define an interface, extend a Spring Data interface, and Spring auto-generates the implementation:

- **`CrudRepository<T, ID>`**:
  - Basic CRUD methods: `save()`, `findById()`, `findAll()`, `deleteById()`.
  - Example:
    ```java
    public interface ProductRepository extends CrudRepository<Product, Long> {}
    ```

- **`JpaRepository<T, ID>`**:
  - Extends `CrudRepository` and adds JPA-specific features:
    - Batch operations
    - `flush()`, `saveAll()`, `findAll(Sort sort)`
    - Paging and sorting
  - Most commonly used in modern apps.

- **`PagingAndSortingRepository<T, ID>`**:
  - Adds methods to retrieve entities using pagination and sorting:
    - `findAll(Pageable pageable)`
    - `findAll(Sort sort)`

### Custom Queries with @Query
For more complex logic, you can use JPQL or native SQL:

- **JPQL (Java Persistence Query Language)** works with entity names:
  ```java
  @Query("SELECT u FROM User u WHERE u.email = ?1")
  User findByEmail(String email);
  ```

- **Native SQL** allows raw SQL queries directly on tables:
  ```java
  @Query(value = "SELECT * FROM users WHERE email = ?1", nativeQuery = true)
  User findByEmailNative(String email);
  ```

- You can also use named parameters:
  ```java
  @Query("SELECT u FROM User u WHERE u.status = :status")
  List<User> findByStatus(@Param("status") String status);
  ```

### Entity Relationships (Mapping)
Spring Data JPA makes it easy to represent relational DB models in Java:

- **@OneToMany** (One entity relates to many of another)
  ```java
  @OneToMany(mappedBy = "user")
  private List<Order> orders;
  ```

- **@ManyToOne** (Many entities relate to one)
  ```java
  @ManyToOne
  @JoinColumn(name = "user_id")
  private User user;
  ```

- **@OneToOne** and **@ManyToMany** are also available for specific modeling scenarios.
- **@JoinColumn** specifies the foreign key column name.

### Fetch Types: Lazy vs Eager
- **LAZY**: Loads data only when accessed.
  - Prevents unnecessary DB calls — better for performance.
  - Default for `@OneToMany`, `@ManyToMany`.

- **EAGER**: Loads data immediately with the parent entity.
  - Useful when data is always needed.
  - Default for `@ManyToOne`, `@OneToOne`.

- You can specify explicitly:
  ```java
  @OneToMany(fetch = FetchType.LAZY)
  private List<Order> orders;
  ```

### Transaction Management with `@Transactional`
- Handles commit/rollback automatically.
- Can be applied at method or class level:
  ```java
  @Service
  public class OrderService {
      @Transactional
      public void placeOrder(Order order) {
          // if any error occurs, transaction will roll back
      }
  }
  ```
- **By default, only unchecked (runtime) exceptions trigger rollback**.
- To rollback for checked exceptions, specify explicitly:
  ```java
  @Transactional(rollbackFor = IOException.class)
  ```

- You can also make a method `readOnly` to optimize performance:
  ```java
  @Transactional(readOnly = true)
  public List<Order> getOrders() {
      return orderRepository.findAll();
  }
  ```

