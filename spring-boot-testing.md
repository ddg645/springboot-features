## ✅ Testing in Spring Boot

### Unit Testing
Unit tests validate individual components like services or controllers in isolation using mock objects.

- **`@WebMvcTest`**:
  - Used for testing only the web layer (controllers).
  - It auto-configures Spring MVC infrastructure and limits loading of other beans.
  - Combine with `@MockBean` to inject mocks into the Spring context.
  ```java
  @WebMvcTest(UserController.class)
  public class UserControllerTest {
      @Autowired
      private MockMvc mockMvc;

      @MockBean
      private UserService userService;

      @Test
      public void testGetUser() throws Exception {
          Mockito.when(userService.getUser(1L)).thenReturn(new User(...));
          mockMvc.perform(get("/users/1"))
              .andExpect(status().isOk());
      }
  }
  ```

- **Mockito** is used to mock dependencies and verify interactions:
  ```java
  when(service.findById(1L)).thenReturn(Optional.of(mockUser));
  verify(service).findById(1L);
  ```

### JPA Testing with `@DataJpaTest`
- Use `@DataJpaTest` for testing repository layer.
- It configures in-memory database (H2 by default), entity scanning, and Spring Data JPA repositories.
  ```java
  @DataJpaTest
  public class UserRepositoryTest {
      @Autowired
      private UserRepository userRepository;

      @Test
      public void testSaveUser() {
          User saved = userRepository.save(new User("John"));
          assertEquals("John", saved.getName());
      }
  }
  ```

### Integration Testing with `@SpringBootTest`
- Loads the full application context.
- Best for verifying end-to-end scenarios.
- Can be used with `@TestRestTemplate` to test REST endpoints:
  ```java
  @SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
  public class UserIntegrationTest {
      @Autowired
      private TestRestTemplate restTemplate;

      @Test
      public void testGetUserEndpoint() {
          ResponseEntity<User> response = restTemplate.getForEntity("/users/1", User.class);
          assertEquals(HttpStatus.OK, response.getStatusCode());
      }
  }
  ```

### MockMvc for Controller Testing
- `MockMvc` allows testing HTTP requests without starting the server.
- Supports fluent API for request building and response assertions.
  ```java
  mockMvc.perform(post("/login").contentType(MediaType.APPLICATION_JSON)
          .content("{\"username\":\"admin\", \"password\":\"1234\"}"))
      .andExpect(status().isOk());
  ```

### Testcontainers for Real Database Testing
- Use [Testcontainers](https://www.testcontainers.org/) to spin up real Docker-based DBs (PostgreSQL, MySQL) during tests.
- Example:
  ```java
  @Testcontainers
  @SpringBootTest
  public class PostgresIntegrationTest {
      @Container
      static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:14")
          .withDatabaseName("testdb")
          .withUsername("test")
          .withPassword("test");

      @DynamicPropertySource
      static void overrideProps(DynamicPropertyRegistry registry) {
          registry.add("spring.datasource.url", postgres::getJdbcUrl);
          registry.add("spring.datasource.username", postgres::getUsername);
          registry.add("spring.datasource.password", postgres::getPassword);
      }
  }
  ```
- Ensures your JPA code is tested against real-world scenarios.

### ✅ Test Coverage Matrix
| Layer            | Purpose                         | Annotation         | Tools/Helpers       | Use Case Example                             |
|------------------|----------------------------------|---------------------|----------------------|-----------------------------------------------|
| Web Layer        | Controller-only testing         | `@WebMvcTest`       | `MockMvc`, `@MockBean` | Test controller routes and status codes       |
| Service Layer    | Business logic                  | N/A                 | `Mockito`            | Test service independently with mock repos    |
| Data Layer       | Repository & JPA testing        | `@DataJpaTest`      | H2 DB, Assertions     | Test query methods, save/find behavior        |
| Integration      | Full context, multiple layers   | `@SpringBootTest`   | `TestRestTemplate`   | Test full API flow and beans interaction      |
| Real DB Testing  | Real containers for DBs         | `@SpringBootTest` + Testcontainers | Docker, JDBC | Test with real PostgreSQL or MySQL containers |

---

### 📁 Recommended Template Project Structure
```
src/
├── main/
│   ├── java/com/example/demo/
│   │   ├── controller/
│   │   ├── service/
│   │   ├── repository/
│   │   ├── model/
│   │   └── DemoApplication.java
│   └── resources/
│       ├── application.yml
│       └── static/
└── test/
    └── java/com/example/demo/
        ├── controller/
        ├── service/
        ├── repository/
        └── integration/
```
- Keep test classes in parallel structure to the source tree.
- Use `@SpringBootTest` in the `integration/` folder.
- Use unit tests for service and controller folders separately.
