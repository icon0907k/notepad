주어진 문자열에서 대문자는 소문자로, 소문자는 대문자로 변환하여 새로운 문자열을 만들어 반환하는 것입니다. 간단하게 설명해볼게요.

문제 설명 :
입력 : 문자열 `my_string`이 주어집니다. 이 문자열은 영어 대문자와 소문자로만 구성되어 있습니다.
출력 : 대문자는 소문자로, 소문자는 대문자로 변환된 새로운 문자열을 반환해야 합니다.

제한사항 :
`my_string`의 길이는 1 이상 1,000 이하입니다.

입출력 예시:
1. 입출력 예 #1
    입력 : `"cccCCC"`
    변환 과정 :
        - 소문자 `c`는 대문자 `C`로 변환되고,
        - 대문자 `C`는 소문자 `c`로 변환됩니다.
    출력 : `"CCCccc"`
2. 입출력 예 #2
    입력 : `"abCdEfghIJ"`
    변환 과정 :
        - 소문자 `a`, `b`, `d`, `e`, `f`, `g`는 각각 대문자로 변환되고,
        - 대문자 `C`, `I`, `J`는 각각 소문자로 변환됩니다.
    출력 : `"ABcDeFGHij"`


``` java
class Solution {
    public String solution(String my_string) {
        
        // 결과 값 변수
        StringBuilder result = new StringBuilder();
        // 소문자 대문자로 대문자 소문자로 변경하기
        for ( char ch : my_string.toCharArray() ) {
            if (Character.isUpperCase(ch)) {
                result.append(Character.toLowerCase(ch));
            } else {
                result.append(Character.toUpperCase(ch));
            }
        }
        return result.toString();
    }
}
```


개인 정리 
1. 문자열의 각 문자를 순회하면서
    1. `Character.isUpperCase()`를 사용하여 대문자인지 확인하고
    2. 대문자라면 `Character.toLowerCase()`로 소문자로 변환
    3. 소문자라면 `Character.toUpperCase()`로 대문자로 변환
2. 변환된 문자들을 모아서 새로운 문자열을 생성한다.
