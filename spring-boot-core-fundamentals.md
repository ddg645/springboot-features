# 🌱 Spring Boot Core Fundamentals (Expanded)

### 1. `@SpringBootApplication`, `@EnableAutoConfiguration` (Under the Hood)

- `@SpringBootApplication` is a meta-annotation that combines:
  - `@Configuration`: Marks the class as a source of bean definitions.
  - `@EnableAutoConfiguration`: Tells Spring Boot to auto-configure beans based on classpath settings, other beans, and various property settings.
  - `@ComponentScan`: Enables component scanning for the package where the class is located.
- ✅ **Interview Insight**: Be ready to explain how Spring Boot finds configurations without XML.

### 2. Auto-Configuration via Classpath Detection

- Spring Boot uses `` to load `AutoConfiguration` classes.
- Example: If `spring-boot-starter-data-jpa` is in the classpath, it configures H2 DB, EntityManager, etc.
- You can override or customize by defining your own `@Bean`:
  ```java
  @Bean
  public DataSource dataSource() {
      // custom datasource setup
  }
  ```

### 3. Spring Boot Starters

- Starters are curated dependency sets:
  - `spring-boot-starter-web`: Spring MVC, Jackson, embedded Tomcat.
  - `spring-boot-starter-data-jpa`: JPA, Hibernate, JDBC.
  - `spring-boot-starter-security`, `spring-boot-starter-test`, etc.

📝 **Tip**: They simplify dependency management and version compatibility.

### 4. Spring Boot CLI

- Run Groovy scripts or Java classes directly from the command line:
  ```bash
  spring run app.groovy
  ```
- Useful for prototypes and microservices.

### 5. Spring Boot DevTools

- Adds features to improve developer productivity:
  - Auto-restart when code changes
  - LiveReload for browser refresh
  - Remote debugging
- Automatically disabled in production.

### 6. Properties and YAML Configuration

#### `application.properties`

```properties
server.port=8081
spring.datasource.url=jdbc:h2:mem:testdb
```

#### `application.yml`

```yaml
server:
  port: 8081
spring:
  datasource:
    url: jdbc:h2:mem:testdb
```

- YAML is preferred for nested config; both are supported.

### 7. Using Profiles

- Environment-specific configuration via `application-{profile}.properties` or YAML.

```properties
spring.profiles.active=dev
```

- Use `@Profile("dev")` on beans to control their loading.

### 8. Embedded Servers

- Comes bundled with:

  - **Tomcat** (default)
  - **Jetty**
  - **Undertow**

- Enables running apps as JARs:

  ```bash
  java -jar app.jar
  ```

- Change server by modifying dependencies in `pom.xml` or `build.gradle`.
