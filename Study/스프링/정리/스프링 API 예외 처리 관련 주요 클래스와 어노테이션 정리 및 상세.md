### 1. **`@RestControllerAdvice`**

- **설명**:
    - `@ControllerAdvice`의 확장판으로, `@RestController`와 결합된 어노테이션입니다.
    - 모든 컨트롤러에서 발생하는 예외를 한 곳에서 처리하며, RESTful API 응답에 적합한 JSON 형태로 결과를 반환합니다.
- **특징**:
    - RESTful 서비스에서 일관된 예외 처리를 제공합니다.
    - 반환 값은 HTTP 메시지 변환기를 통해 JSON 형식으로 변환됩니다.
- **예제**:
```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(IllegalArgumentException ex) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(ex.getMessage());
    }
}
```
---
### 2. **`HandlerExceptionResolver`**

- **설명**:
    - 스프링 MVC에서 예외를 처리하기 위한 인터페이스입니다.
    - 예외를 해결하고 적절한 HTTP 응답을 제공하는 역할을 합니다.
- **특징**:
    - 예외를 가로채고, 커스텀 로직으로 처리할 수 있습니다.
    - `resolveException` 메서드를 구현해야 합니다.
- **사용 예제**:
```java
public class CustomHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        try {
            response.sendError(HttpStatus.INTERNAL_SERVER_ERROR.value(), ex.getMessage());
        } catch (IOException e) {
            e.printStackTrace();
        }
        return new ModelAndView();
    }
}
```
---
### 3. **`ExceptionResolver`**

- **설명**:
    - `HandlerExceptionResolver`의 부모 인터페이스로, 예외 처리 메커니즘의 핵심 인터페이스입니다.
    - 스프링 MVC가 내부적으로 다양한 `HandlerExceptionResolver`를 등록해 예외를 처리합니다.
- **특징**:
    - 추상적인 역할만 담당하며, 구체적인 구현체를 통해 동작합니다.
    - 직접 사용하는 경우는 드뭅니다.
---
### 4. **`ResponseStatusExceptionResolver`**

- **설명**:
    - 예외 클래스에 `@ResponseStatus`가 지정된 경우, 이를 해석하여 HTTP 응답 상태 코드를 설정합니다.
- **특징**:
    - 사용자 정의 예외와 HTTP 상태 코드를 연결할 때 사용됩니다.
    - 예외를 자동으로 HTTP 상태 코드와 매핑합니다.
- **예제**:
```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```
---
### 5. **`DefaultHandlerExceptionResolver`**

- **설명**:
    - 스프링에서 기본으로 제공하는 예외 처리기입니다.
    - 스프링 MVC의 표준 예외(예: `HttpRequestMethodNotSupportedException`, `TypeMismatchException`)를 처리합니다.
- **특징**:
    - 스프링의 표준 동작을 기반으로 하며, 사용자 정의 동작이 필요하면 다른 Resolver를 등록하거나 커스터마이징합니다.
    - 우선순위가 낮아, 커스텀 Resolver가 먼저 실행됩니다.
----
### 6. **`@ExceptionHandler`**

- **설명**:
    - 특정 컨트롤러에서 발생한 특정 예외를 처리하기 위한 어노테이션입니다.
    - 예외를 처리하는 메서드에 붙여 사용합니다.
- **특징**:
    - 처리 대상 예외를 메서드 매개변수로 전달받을 수 있습니다.
    - 컨트롤러 클래스 또는 `@ControllerAdvice` 클래스에서 사용 가능합니다.
- **예제**:
``` java
@RestController
public class SampleController {
    @GetMapping("/test")
    public String test() {
        throw new IllegalArgumentException("Invalid input!");
    }
    
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(IllegalArgumentException ex) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(ex.getMessage());
    }
}
```
---
### 7. **`@ControllerAdvice`**

- **설명**:
    - 모든 컨트롤러에서 발생하는 예외를 한 곳에서 처리하기 위한 어노테이션입니다.
    - `@RestControllerAdvice`와 달리, HTML 뷰를 반환하는 컨트롤러에서 주로 사용됩니다.
- **특징**:
    - 공통적인 예외 처리 로직을 중앙에서 관리할 수 있습니다.
    - RESTful API를 제외한 일반적인 웹 애플리케이션에서 적합합니다.
- **예제**:
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public String handleException(Exception ex, Model model) {
        model.addAttribute("error", ex.getMessage());
        return "error-page";
    }
}
```
---
### 주요 차이점 요약

|기능|@RestControllerAdvice|@ControllerAdvice|@ExceptionHandler|HandlerExceptionResolver|
|---|---|---|---|---|
|대상|RESTful API|일반 MVC|특정 컨트롤러|전역 예외 처리|
|반환 값|JSON|HTML 뷰|JSON/HTML|유연한 처리|
|적용 범위|전역|전역|컨트롤러별|전역|

### 스프링 예외 처리 흐름

1. **`HandlerExceptionResolver`**는 스프링 MVC에서 예외를 가로채는 핵심 역할을 합니다.
2. **`@ExceptionHandler`**는 컨트롤러별 예외 처리 로직을 제공합니다.
3. **`@ControllerAdvice`/`@RestControllerAdvice`**는 전역 예외 처리 기능을 제공합니다.
4. 스프링의 기본 동작은 **`DefaultHandlerExceptionResolver`**와 **`ResponseStatusExceptionResolver`**에 의해 처리됩니다.
---
## 예외 처리 등록: 어디에, 왜 하는지

#### **1. 자동 등록되는 경우**

- **어디에 등록되는가?**
    - 스프링 컨텍스트(ApplicationContext)에 자동으로 등록됩니다.
    - 스프링 부트가 실행되면서 `@RestControllerAdvice`, `@ControllerAdvice`, `@ExceptionHandler`와 같은 어노테이션 기반 예외 처리 기능이 스캔되고, 스프링 컨텍스트에 빈으로 등록됩니다.
    - 기본 제공 `ResponseStatusExceptionResolver`, `DefaultHandlerExceptionResolver` 등은 스프링 MVC의 기본 설정으로 자동 등록됩니다.
- **왜 등록하는가?**
    - 스프링은 예외 처리 메커니즘을 일관되게 관리하기 위해, 예외 처리 핸들러를 스프링 컨텍스트에 등록합니다.
    - 이를 통해 특정 예외 상황에서 호출될 핸들러를 자동으로 매핑하고, 일관된 응답을 제공할 수 있습니다.

---
#### **2. 명시적으로 등록해야 하는 경우**

- **어디에 등록하는가?**
    - 직접 구현한 `HandlerExceptionResolver`를 등록하려면 `WebMvcConfigurer`를 사용하거나, 빈으로 등록합니다.
    - 예를 들어, 아래와 같은 방식으로 등록합니다:
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        resolvers.add(new CustomHandlerExceptionResolver());
    }
}
```
```java
@Bean
public HandlerExceptionResolver customHandlerExceptionResolver() {
    return new CustomHandlerExceptionResolver();
}
```
**왜 등록하는가?**
- 스프링의 기본 예외 처리 흐름을 확장하거나 대체하기 위해 필요합니다.
- 특정 요구사항(예: 예외 로깅, 커스텀 응답 포맷 등)을 처리하려면 기본 설정만으로는 부족할 수 있습니다.
- `HandlerExceptionResolver`를 명시적으로 등록하면, 기본 처리기를 우선순위에 따라 덮어쓸 수 있습니다.
---
### 예외 처리 자동 등록 요약

| 기능/클래스                            | 자동 등록 여부          | 명시적 등록 필요 여부 |
| --------------------------------- | ----------------- | ------------ |
| `@RestControllerAdvice`           | O (자동 등록)         | X            |
| `@ControllerAdvice`               | O (자동 등록)         | X            |
| `@ExceptionHandler`               | O (자동 등록)         | X            |
| `HandlerExceptionResolver`        | X (직접 구현 시 등록 필요) | O            |
| `ResponseStatusExceptionResolver` | O (자동 등록)         | X            |
| `DefaultHandlerExceptionResolver` | O (자동 등록)         | X            |

---
### 스프링에서 예외 처리 등록 흐름

1. **스프링 부트 초기화**:
    - `@RestControllerAdvice`, `@ControllerAdvice`, `@ExceptionHandler`는 자동으로 등록됩니다.
2. **스프링 MVC 기본 설정**:
    - `ResponseStatusExceptionResolver`, `DefaultHandlerExceptionResolver` 등은 스프링이 기본으로 등록합니다.
3. **커스터마이징 필요 시**:
    - `HandlerExceptionResolver`를 구현하거나, `WebMvcConfigurer`를 통해 등록해야 합니다.
---