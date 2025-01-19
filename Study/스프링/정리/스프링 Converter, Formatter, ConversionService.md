`Converter`, `Formatter`, `ConversionService`는 Spring Framework에서 데이터 변환과 형식화(formatting)를 처리하기 위한 인터페이스와 클래스를 제공합니다. 이들은 주로 웹 애플리케이션의 입력 처리, 데이터 바인딩, 데이터 표시와 같은 작업에 사용됩니다.

---
### 1. **Converter**
- **정의**: 한 타입의 객체를 다른 타입으로 변환하는 데 사용되는 간단한 인터페이스입니다.
- **인터페이스**: `org.springframework.core.convert.converter.Converter`
- **주요 메서드**:
    `T convert(S source);`
    - `S`는 소스 타입, `T`는 대상 타입입니다.
- **사용 사례**:
    - 문자열을 특정 객체로 변환 (`String -> LocalDateTime`)
    - 사용자 정의 객체 변환 (`CustomType1 -> CustomType2`)

#### 예시:
```java
public class StringToLocalDateTimeConverter implements Converter<String, LocalDateTime> {
    @Override
    public LocalDateTime convert(String source) {
        return LocalDateTime.parse(source, DateTimeFormatter.ISO_DATE_TIME);
    }
}
```
---

### 2. **Formatter**

- **정의**: 문자열과 객체 간의 변환을 처리하며, 주로 데이터를 특정 형식으로 변환하거나 포맷을 처리합니다.
- **인터페이스**: `org.springframework.format.Formatter`
- **주요 메서드**:
```java
String print(T object, Locale locale); // 객체를 문자열로 변환
T parse(String text, Locale locale);  // 문자열을 객체로 변환
```
    
- **사용 사례**:
    - 숫자, 날짜, 통화와 같은 형식화가 필요한 데이터를 처리 (`Double -> String`, `String -> Double`)
    - 특정 로케일을 기반으로 형식화 처리

#### 예시:
```java
public class LocalDateFormatter implements Formatter<LocalDate> {
    @Override
    public LocalDate parse(String text, Locale locale) {
        return LocalDate.parse(text, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
    }
    
    @Override
    public String print(LocalDate object, Locale locale) {
        return object.format(DateTimeFormatter.ofPattern("yyyy-MM-dd"));
    }
}
```

---

### 3. **ConversionService**

- **정의**: 여러 `Converter`를 등록하고 관리하여 타입 변환을 제공하는 인터페이스입니다.
- **인터페이스**: `org.springframework.core.convert.ConversionService`
- **주요 메서드**:
```java
boolean canConvert(Class<?> sourceType, Class<?> targetType);
<T> T convert(Object source, Class<T> targetType);
```
    
- **특징**:
    - 중앙에서 변환 로직을 관리하고 재사용성을 높임
    - 스프링이 기본적으로 제공하는 `DefaultConversionService`나 `GenericConversionService`를 확장하여 커스텀 변환기를 등록 가능
- **사용 사례**:
    - 여러 변환기를 통합하여 사용
    - 데이터 바인딩 시 자동 변환 처리

#### 예시:
```java
package hello.typeconverter.formatter;

import hello.typeconverter.converter.IpPortToStringConverter;
import hello.typeconverter.converter.StringToIpPortConverter;
import hello.typeconverter.type.IpPort;
import org.junit.jupiter.api.Test;
import org.springframework.format.support.DefaultFormattingConversionService;
import static org.assertj.core.api.Assertions.assertThat;

public class FormattingConversionServiceTest {
    @Test
    void formattingConversionService() {
        DefaultFormattingConversionService conversionService = new DefaultFormattingConversionService();
        // 컨버터 등록
        conversionService.addConverter(new StringToIpPortConverter());
        conversionService.addConverter(new IpPortToStringConverter());
        // 포맷터 등록
        conversionService.addFormatter(new MyNumberFormatter());
        // 컨버터 사용
        IpPort ipPort = conversionService.convert("127.0.0.1:8080", IpPort.class);
        assertThat(ipPort).isEqualTo(new IpPort("127.0.0.1", 8080));
        // 포맷터 사용
        assertThat(conversionService.convert(1000, String.class)).isEqualTo("1,000");
        assertThat(conversionService.convert("1,000", Long.class)).isEqualTo(1000L);
    }
}
```

---
### **Converter와 Formatter의 차이점**

| **항목**     | **Converter**     | **Formatter**          |
| ---------- | ----------------- | ---------------------- |
| **주요 목적**  | 타입 간 변환           | 객체와 문자열 간 변환 및 포맷 처리   |
| **사용 위치**  | 일반적인 타입 변환 작업     | 데이터 형식화와 문자열 변환 작업     |
| **로케일 지원** | 지원하지 않음           | 로케일을 지원 (e.g., 날짜 형식화) |
| **인터페이스**  | `Converter<S, T>` | `Formatter<T>`         |
| **적용 방식**  | 변환만 담당            | 변환 및 형식화               |

---
### 정리
- **Converter**는 일반적인 변환 작업을 위한 단순한 인터페이스.
- **Formatter**는 문자열과 객체 간의 변환 및 포맷을 처리하며, 로케일 지원이 가능.
- **ConversionService**는 `Converter`와 `Formatter`를 중앙에서 관리하며 통합적으로 사용하도록 도와줍니다.

---
### `Converter` 등록하기
`Converter`는 객체 간 변환을 처리하며, `ConversionService`에 등록하여 사용할 수 있습니다. 이를 통해 Spring MVC가 요청 파라미터를 변환할 때 자동으로 커스텀 변환기를 사용합니다.
#### 등록 방법:
- **`WebMvcConfigurer`**를 구현하여 **`addFormatters`** 메서드를 오버라이드하고, **`ConversionService`**에 커스텀 **`Converter`**를 등록합니다.
#### 예제:
```java
@Configuration
public class ConverterConfig implements WebMvcConfigurer {
    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addConverter(new StringToLocalDateTimeConverter()); // String -> LocalDateTime 변환기 등록
    }
}
```

---
### 2. `Formatter` 등록하기
`Formatter`는 문자열과 객체 간의 변환과 로케일에 맞춘 형식화 작업에 사용됩니다. `FormattingConversionService`에 등록하여 사용합니다.
#### 등록 방법:
- `WebMvcConfigurer`에서 `addFormatters`메서드를 오버라이드하여 `Formatter를 등록합니다.
#### 예제:
```java
@Configuration
public class FormatterConfig implements WebMvcConfigurer {

    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addFormatter(new LocalDateFormatter()); // LocalDate -> String 변환기 등록
    }
}
```

---
### 3. `ConversionService` 등록하기
`ConversionService`는 `Converter`와 `Formatter`를 모두 관리하는 서비스입니다. `FormattingConversionService`를 사용하여 두 가지를 함께 등록하고, 타입 변환을 중앙에서 관리할 수 있습니다.
#### 등록 방법:
- **`FormattingConversionService`**를 사용하여 **`Converter`**와 **`Formatter`**를 함께 등록합니다.
#### 예제:
```java
@Configuration
public class ConversionConfig {

    @Bean
    public FormattingConversionService conversionService() {
        FormattingConversionService conversionService = new FormattingConversionService();
        conversionService.addConverter(new StringToLocalDateTimeConverter()); // Converter 등록
        conversionService.addFormatter(new LocalDateFormatter()); // Formatter 등록
        return conversionService;
    }
}
```

---
### 4. **컨트롤러에서의 활용**
`Converter`와 `Formatter`를 등록한 후, **Spring MVC**는 자동으로 요청 파라미터를 변환하여 **컨트롤러 메서드**의 파라미터로 바인딩합니다.
#### 예시:
```java
@RestController
public class ExampleController {

    @GetMapping("/convert-example")
    public String convertExample(@RequestParam("date") LocalDateTime date) {
        return "Converted Date: " + date.toString();
    }
}
```
- **요청**: `GET /convert-example?date=2025-01-19T10:15:30`
- **결과**: `"Converted Date: 2025-01-19T10:15:30"`