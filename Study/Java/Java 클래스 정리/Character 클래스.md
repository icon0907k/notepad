`Character`는 Java에서 문자를 다루기 위한 클래스로 기본 데이터 타입인 `char`를 감싸고 있는 래퍼 클래스이다. 이 클래스는 문자와 관련된 여러 유용한 메소드를 제공한다.

**주요 기능**

문자 판별 메소드
1. `Character.isDigit(char ch)`
    주어진 문자가 숫자인지 확인하는 메소드
    예: `'5'`는 숫자 → `true`, `'a'`는 숫자가 아님 → `false`
2. `Character.isLetter(char ch)`
    주어진 문자가 알파벳 문자인지 확인
    예: `'a'`는 알파벳 → `true`, `'5'`는 알파벳 아님 → `false`
3. `Character.isLetterOrDigit(char ch)` 
    주어진 문자가 알파벳이거나 숫자인지 확인
    예: `'a'`, `'5'` 둘 다 → `true`, `'!'`는 → `false`
4. `Character.isLowerCase(char ch)`  
    주어진 문자가 소문자인지 확인
    예: `'a'`는 소문자 → `true`, `'A'`는 → `false`
5. `Character.isUpperCase(char ch)`
    주어진 문자가 대문자인지 확인
    예: `'A'`는 대문자 → `true`, `'a'`는 → `false`
6. `Character.isWhitespace(char ch)`
    공백 문자인지 확인 (예를 들어 빈칸, 탭 등)  
    예: `' '` (빈칸)은 공백 → `true`, `'a'`는 → `false`
7. `Character.isDefined(char ch)`
    유니코드에서 정의된 문자면 `true`를 반환
    거의 모든 문자와 기호가 유니코드에서 정의되어 있어서 잘 쓰이진 않음    
8. `Character.isISOControl(char ch)`
    제어 문자(보이지 않는 특수 문자)인지 확인
    

문자 변환 메소드
1. `Character.toUpperCase(char ch)`
    소문자를 대문자로 
    예: `'a'` → `'A'`, `'b'` → `'B'`
2. `Character.toLowerCase(char ch)`
    대문자를 소문자로 
    예: `'A'` → `'a'`, `'B'` → `'b'`
    

숫자 변환 메소드
1. `Character.getNumericValue(char ch)`
    문자를 숫자로 변환
    예: `'5'` → `5`, `'a'` → 알파벳 순서 값 (ex: 10)
2. `Character.digit(char ch, int radix)`
    주어진 진법(radix, 2진법, 10진법 등)에 맞춰 문자를 숫자로 변환  
    예: `'1'`, 10진법 → `1`, `'A'`, 16진법 → `10`

코드 포인트 관련 메소드
1. `Character.codePointAt(CharSequence seq, int index)`
    문자열에서 특정 위치의 유니코드 코드 포인트를 반환
    예: `"Hello"`에서 인덱스 1에 있는 문자 `'e'`의 코드 포인트를 반환
2. `Character.charCount(int codePoint)`
    주어진 코드 포인트가 몇 개의 `char`로 표현 (1 또는 2) 2는 특수 문자나 일부 이모지 인 경우
3. `Character.toChars(int codePoint)`
    코드 포인트를 `char` 배열로 변환

기타 메소드
1. `Character.isTitleCase(char ch)`
    타이틀 케이스(제목에서 첫 글자만 대문자인 형식)인지 확인
2. `Character.reverseBytes(char ch)`
    문자의 바이트 순서를 반대로 (자주 쓰이는 메소드는 아님)
3. `Character.MIN_VALUE`, `Character.MAX_VALUE`
    `char`의 최소값과 최대값을 나타내. 최소값은 `'\u0000'`, 최대값은 `'\uffff'` (유니코드 범위).
4. `Character.isSurrogate(char ch)`
    주어진 문자가 서러게이트 문자(유니코드의 특수 문자)를 나타내는지 확인해.

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