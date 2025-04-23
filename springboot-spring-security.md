
## 🔐 Security with Spring Boot

### Spring Security Basics
- Spring Security is the de facto standard for securing Spring applications.
- It provides authentication, authorization, and protection against common attacks like CSRF, session fixation, and clickjacking.
- It works by inserting a chain of servlet filters into the application.

### Securing Endpoints
- You can secure specific endpoints using configuration or annotations:
  ```java
  @RestController
  public class AdminController {
      @GetMapping("/admin")
      @PreAuthorize("hasRole('ADMIN')")
      public String adminPanel() {
          return "Admin Access";
      }
  }
  ```

### Configuring SecurityFilterChain (Spring Security 6+)
- In Spring Security 6, the XML or `WebSecurityConfigurerAdapter` approach is replaced with a `SecurityFilterChain` bean:
  ```java
  @Bean
  public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
      return http
          .csrf().disable()
          .authorizeHttpRequests(auth -> auth
              .requestMatchers("/public/**").permitAll()
              .anyRequest().authenticated()
          )
          .httpBasic(Customizer.withDefaults())
          .build();
  }
  ```

### JWT Authentication Workflow
- **JWT (JSON Web Token)** is used for stateless authentication.
- Flow:
  1. User logs in → gets a signed JWT token
  2. Token is sent with each request in the `Authorization` header
  3. Server validates the token signature and parses the claims

- Typical setup:
  - A custom `UsernamePasswordAuthenticationFilter`
  - A JWT utility for creating/validating tokens
  - A filter to intercept requests and extract token information

### Method-Level Security
- You can use annotations to control method-level access:
  - `@PreAuthorize("hasRole('ADMIN')")`: Uses SpEL (Spring Expression Language)
  - `@Secured("ROLE_ADMIN")`: Simpler, but less flexible

- Enable via:
  ```java
  @EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
  ```

### OAuth2 and Single Sign-On (SSO)
- Spring Boot supports OAuth2 for third-party authentication (Google, GitHub, etc.).
- Add dependency:
  ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
  </dependency>
  ```

- Example config for GitHub login:
  ```yaml
  spring:
    security:
      oauth2:
        client:
          registration:
            github:
              client-id: YOUR_CLIENT_ID
              client-secret: YOUR_SECRET
              scope: read:user
          provider:
            github:
              authorization-uri: https://github.com/login/oauth/authorize
              token-uri: https://github.com/login/oauth/access_token
              user-info-uri: https://api.github.com/user
  ```
- Spring Security handles the full login redirect flow and session creation.
