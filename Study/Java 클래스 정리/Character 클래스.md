`Character`는 Java에서 문자를 다루기 위한 클래스로 기본 데이터 타입인 `char`를 감싸고 있는 래퍼 클래스이다. 이 클래스는 문자와 관련된 여러 유용한 메소드를 제공한다.

**주요 기능**
1. **문자 판별**:
    1. `Character.isDigit(char ch)`: 주어진 문자가 숫자인지 판별.
    2. `Character.isLetter(char ch)`: 문자가 알파벳인지 판별.
    3. `Character.isWhitespace(char ch)`: 문자가 공백인지 판별.
2. **문자 변환**:
    1. `Character.toUpperCase(char ch)`: 주어진 문자를 대문자로 변환.
    2. `Character.toLowerCase(char ch)`: 주어진 문자를 소문자로 변환.
3. **문자형 변환**:
    1. `Character.getNumericValue(char ch)`: 문자 '0'~'9'에 해당하는 숫자값을 반환. '5'는 5로 변환
4. **상수**:
    1. `Character.MIN_VALUE`: `char`의 최소값(유니코드에서 가장 낮은 값).
    2. `Character.MAX_VALUE`: `char`의 최대값(유니코드에서 가장 높은 값).

**사용 예시**

``` java
public class CharacterExample {
    public static void main(String[] args) {
        char ch = '5';
        
        // 숫자인지 확인
        if (Character.isDigit(ch)) {
            System.out.println(ch + "는 숫자입니다.");
        }
        // 대문자로 변환
        char upper = Character.toUpperCase('a');
        System.out.println("대문자: " + upper);
        // 숫자값 변환
        int numericValue = Character.getNumericValue(ch);
        System.out.println("숫자값: " + numericValue);
    }
}
```

`Character` 클래스는 문자와 관련된 다양한 기능을 제공하여 문자열 처리, 문자 검증, 문자 변환 등을 쉽게 할 수 있도록 돕는다. 이 클래스는 Java에서 문자를 다루는 데 유용하다.