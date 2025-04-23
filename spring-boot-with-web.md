
## 🌐 Spring Boot with Web (REST APIs)

### REST Annotations
- `@RestController`: Combines `@Controller` and `@ResponseBody` to simplify RESTful controllers.
- `@RequestMapping`: Maps HTTP requests to handler methods.
- `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`: Shorthand for common HTTP methods.

### Request Mapping Annotations
- `@RequestParam`: Extracts query parameters.
- `@PathVariable`: Binds URI path variables.
- `@RequestBody`: Maps the request body to a method parameter, usually for JSON input.

### Exception Handling using `@ControllerAdvice`
- Use a centralized error handler class annotated with `@ControllerAdvice`.
- Define methods annotated with `@ExceptionHandler` to handle specific exceptions:
  ```java
  @ExceptionHandler(ResourceNotFoundException.class)
  public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
      return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
  }
  ```

### Content Negotiation
- Spring Boot automatically handles content negotiation based on the `Accept` header.
- Can return JSON or XML depending on request.
- Jackson (for JSON) and JAXB (for XML) are used under the hood.

### Validation using `@Valid`, `@NotNull`, `@Size`, etc.
- Use JSR-380 (javax.validation) annotations in request DTOs.
- Combine with `@Valid` in controller methods:
  ```java
  public ResponseEntity<Void> createUser(@Valid @RequestBody UserDto dto) {...}
  ```
- Errors can be captured via `BindingResult` or `@ExceptionHandler(MethodArgumentNotValidException.class)`.

### Global CORS Config via `WebMvcConfigurer`
- Allow cross-origin requests from frontends (React, Angular, etc.)
- Implement `WebMvcConfigurer`:
  ```java
  @Configuration
  public class WebConfig implements WebMvcConfigurer {
      @Override
      public void addCorsMappings(CorsRegistry registry) {
          registry.addMapping("/**")
                  .allowedOrigins("http://localhost:3000")
                  .allowedMethods("GET", "POST", "PUT", "DELETE");
      }
  }
  ```

