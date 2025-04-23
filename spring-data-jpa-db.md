## 🗃️ Spring Data JPA & Database Integration

### Repository Interfaces
- **`CrudRepository`**: Basic CRUD operations.
- **`JpaRepository`**: Adds JPA-specific methods like `flush()`, batch delete, and pagination.
- **`PagingAndSortingRepository`**: Adds pagination and sorting methods.
- Example:
  ```java
  public interface UserRepository extends JpaRepository<User, Long> {}
  ```

### Custom Queries
- Use `@Query` for custom JPQL or native SQL:
  ```java
  @Query("SELECT u FROM User u WHERE u.status = ?1")
  List<User> findByStatus(String status);

  @Query(value = "SELECT * FROM users WHERE status = ?1", nativeQuery = true)
  List<User> findByStatusNative(String status);
  ```
- JPQL works with entity names, while native queries work with actual table/column names.

### Entity Mapping
- Relationships:
  - `@OneToMany`, `@ManyToOne`, `@OneToOne`, `@ManyToMany`
  - Example:
    ```java
    @OneToMany(mappedBy = "user")
    private List<Order> orders;

    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
    ```

### Fetch Types
- **LAZY**: Data is loaded only on access (default for `@OneToMany`).
- **EAGER**: Data is loaded immediately (default for `@ManyToOne`).
- Prefer LAZY to avoid performance issues.

### Transactions
- Use `@Transactional` to define method-level or class-level transactions.
  ```java
  @Transactional
  public void updateOrderStatus(Long id) {
      // transactional logic
  }
  ```
- Rollback occurs for `RuntimeException` by default. Customize via:
  ```java
  @Transactional(rollbackFor = CustomException.class)
  ```

