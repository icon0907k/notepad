주어진 문자열 `my_string`에서 모든 자연수(1자리 숫자)의 합을 계산하는 `solution` 함수를 완성하세요.

 제한 사항
1. `1 ≤ my_string의 길이 ≤ 1,000`
2. `my_string`은 소문자, 대문자, 그리고 한자리 자연수로만 구성되어 있습니다.

 입출력 예시

1. 입출력 예 #1
    1. 입력 : `"aAb1B2cC34oOp"`
    2. 출력 : `10`
    3. 설명 : 문자열 내의 한자리 자연수는 1, 2, 3, 4입니다. 따라서 1 + 2 + 3 + 4 = 10을 반환합니다.
2. 입출력 예 #2
    1. 입력 : `"1a2b3c4d123Z"`
    2. 출력 : `16`
    3. 설명 : 문자열 내의 한자리 자연수는 1, 2, 3, 4, 1, 2, 3입니다. 따라서 1 + 2 + 3 + 4 + 1 + 2 + 3 = 16을 반환합니다.

해결 방법
1. 문자열을 순회하여 각 문자를 확인합니다.
2. 문자가 숫자인 경우 해당 숫자를 정수로 변환하여 합계에 더합니다.
3. 최종적으로 합계를 반환합니다.


``` java
class Solution {
    public int solution(String my_string) {
        int sum = 0; // 합계를 저장할 변수
        
        // 문자열의 각 문자를 확인
        for (int i = 0; i < my_string.length(); i++) {
            char ch = my_string.charAt(i);
            
            // 문자가 숫자일 경우
            if (Character.isDigit(ch)) {
                sum += Character.getNumericValue(ch); // 숫자로 변환 후 더하기
            }
        }
        
        return sum; // 최종 합계 반환
    }
}
```


개인 정리 
1. 문자열 메소드 charAt() 사용한다.  
2. Character 클래스를 사용하여 isDigit() 메소드를 이용 숮자인지 확인한다.
3. Character 클래스 getNumericValue() 메소드를 이용 문자 값을 숫자로 변환 후 sum 변수 값에 더한다.