# 🌱 Spring Boot Features Guide

Welcome to the ultimate Spring Boot guide! This README covers everything you need to master Spring Boot — from fundamentals to advanced real-world practices.

---

## 📘 Table of Contents

1. [Spring Boot Core Fundamentals](#1-spring-boot-core-fundamentals)
2. [Spring Boot with Web (REST APIs)](#2-spring-boot-with-web-rest-apis)
3. [Spring Data JPA & Database Integration](#3-spring-data-jpa--database-integration)
4. [Security with Spring Boot](#4-security-with-spring-boot)
5. [Testing in Spring Boot](#5-testing-in-spring-boot)
6. [Spring Boot Actuator & Monitoring](#6-spring-boot-actuator--monitoring)
7. [Spring Boot & DevOps Readiness](#7-spring-boot--devops-readiness)
8. [Advanced Concepts](#8-advanced-concepts)
9. [Real-world Patterns and Best Practices](#9-real-world-patterns-and-best-practices)

---

## 1. Spring Boot Core Fundamentals

- `@SpringBootApplication`, `@EnableAutoConfiguration` under the hood
- Auto Configuration via classpath detection
- Spring Boot Starters like:
  - `spring-boot-starter-web`
  - `spring-boot-starter-data-jpa`
- Spring Boot CLI for rapid testing
- DevTools for hot reload and live refresh
- Properties and YAML configuration
  - `application.properties` vs `application.yml`
  - Using Profiles (`@Profile`, `spring.profiles.active`)
- Embedded Servers (Tomcat, Jetty, Undertow)

---

## 2. Spring Boot with Web (REST APIs)

- REST Annotations:
  - `@RestController`, `@RequestMapping`, `@GetMapping`, etc.
  - `@RequestBody`, `@PathVariable`, `@RequestParam`
- Exception Handling using `@ControllerAdvice`
- Content Negotiation: JSON/XML responses
- Validation using `@Valid`, `@NotNull`, `@Size`
- Global CORS config via `WebMvcConfigurer`

---

## 3. Spring Data JPA & Database Integration

- Repository Interfaces:
  - `CrudRepository`, `JpaRepository`, `PagingAndSortingRepository`
- Custom Queries:
  - JPQL, native queries, `@Query`
- Entity Mapping:
  - Relationships: `@OneToMany`, `@ManyToOne`, `@JoinColumn`
  - Fetch types: `FetchType.LAZY` vs `EAGER`
- Transactions: `@Transactional`, rollback conditions

---

## 4. Security with Spring Boot

- Spring Security Basics
  - Securing endpoints, configuring access
  - Configuring `SecurityFilterChain` (Spring Security 6+)
- JWT Authentication Workflow
- `@PreAuthorize`, `@Secured` method security
- OAuth2 and Single Sign-On (SSO)

---

## 5. Testing in Spring Boot

- Unit Tests:
  - `@WebMvcTest`, `@MockBean`, Mockito
- JPA Tests:
  - `@DataJpaTest`, in-memory H2 DB
- Integration Testing:
  - `@SpringBootTest`, `@TestRestTemplate`
- MockMvc for controller testing
- Testcontainers for real DB interaction

---

## 6. Spring Boot Actuator & Monitoring

- Built-in Endpoints:
  - `/actuator/health`, `/metrics`, `/info`, `/env`, etc.
- Custom Endpoints via `@Endpoint`
- Integration with:
  - Prometheus, Grafana, ELK stack

---

## 7. Spring Boot & DevOps Readiness

- Dockerizing Spring Boot apps with `Dockerfile`
- Deployment on Kubernetes (Spring Boot with Helm or Kustomize)
- CI/CD Pipelines (GitHub Actions, Jenkins, GitLab)
- Externalized config with profiles and secrets

---

## 8. Advanced Concepts

- Async Processing:
  - `@Async`, `CompletableFuture`
- Scheduled Jobs:
  - `@Scheduled(fixedRate = ...)`
- Domain Events:
  - `ApplicationEventPublisher`
- Spring Cloud (optional for microservices):
  - Config Server, Eureka, Feign, Gateway, Resilience4j

---

## 9. Real-world Patterns and Best Practices

- Clean Architecture and Modularization
- DTO vs Entity Separation
- Service/Repository Pattern
- Global Exception Handling
- Logging with SLF4J, Logback, JSON logs for ELK
- API Versioning strategies
- API Rate Limiting & Throttling

---

⭐ **Star this repo if it helped you!**

