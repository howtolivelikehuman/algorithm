## 8-15 문자열 압축 (프로그래머스) [JAVA] 2020 KAKAO BLIND RECRUITMENT

#### 문제

데이터 처리 전문가가 되고 싶은 **어피치**는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.
간단한 예로 aabbaccc의 경우 2a2ba3c(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 예를 들면, abcabcdede와 같은 문자열은 전혀 압축되지 않습니다. 어피치는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.

예를 들어, ababcdcdababcdcd의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 2ab2cd2ab2cd로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 2ababcdcd로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.

다른 예로, abcabcdede와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 abcabc2de가 되지만, 3개 단위로 자른다면 2abcdede가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.

압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.

**제한사항**

- s의 길이는 1 이상 1,000 이하입니다.
- s는 알파벳 소문자로만 이루어져 있습니다.

##### **입출력 예**

| s                            | result |
| ---------------------------- | ------ |
| `"aabbaccc"`                 | 7      |
| `"ababcdcdababcdcd"`         | 9      |
| `"abcabcdede"`               | 8      |
| `"abcabcabcabcdededededede"` | 14     |
| `"xababcdcdababcdcd"`        | 17     |

#### 풀이

단순하게 풀었다. 완전탐색이 아니면 풀 수 없다고 생각했다.

우선 문자열을 분석할때 두 가지 경우로 나누었다.

1. 문자열이 같은 경우
2. 문자열이 다른 경우

문자열이 같은 경우엔 최초에 숫자 한글자가 추가된다.

(여기서 한참 해매었던게 두자리 수 가 되면 숫자가 두글자로 하나 더 늘어난다)

문자열이 다른 경우엔 그 문자열 만큼 (range) 더해준다.

그리고 이제 마지막으로 나머지 문자열을 더해주면 될 것이라고 생각했다.



이제 구현을 하며 기준 문자열의 index를 계속 저장하고, 문자열이 달라지면 (압축이 끝나면) index를 바꾸는 형식으로 구현했다.

여기서 전체 문자열의 길이의 반만큼 전체 알고리즘을 돌리고 최소값을 구하였다.



#### 코드

````java
class Solution {
	int temp = 0;
	int answer = 0;
	String s;
    public int solution(String s) {
        this.s = s;
        answer = s.length();
       for(int i=1; i<=s.length()/2; i++) {
    	   compress(i);
       }
       System.out.println(answer);
       return answer;
    }
    
    public void compress(int range) {
    	temp = 0;
    	char[] ss = s.toCharArray();
    	int i=0;
    	int compareIndex = 0;
    	int compareCount = 0;
    	
    	for(i=range; i <= s.length()-range; i = i+range) {
    		
    		//만약 같음
    		if(compare(ss,compareIndex,i,range)) {
    			compareCount++;
    			if(compareCount == 1) {
    				temp = temp+1; //숫자표기용
    			}
    			else if(compareCount == 9) {
    				temp = temp+1; //두자리수 됨
    			}
    		}
    		//만약 다름
    		else {
    			//비교초기화
    			compareIndex = i;
    			compareCount = 0; 
    			
    			//문자열만큼 길이 추가
    			temp = temp+range;
    		}
    	}
        //나머지 문자열 더해주기
    	temp = temp -  i + s.length() + range;

    	//System.out.println("range : " + range + " " + temp);
    	answer = Math.min(temp, answer);
    }
    
    public boolean compare(char[] ss, int compInx, int x, int range) {
    	
    	for(int i=0; i<range; i++) {
    		if(ss[compInx+i] != ss[x+i])
    			return false;
    	}
    	
    	return true;
    }
}
````

#### 결과

````
테스트 1 〉	통과 (0.28ms, 53.4MB)
테스트 2 〉	통과 (1.35ms, 52.2MB)
테스트 3 〉	통과 (0.65ms, 55.3MB)
테스트 4 〉	통과 (0.18ms, 52.7MB)
테스트 5 〉	통과 (0.22ms, 52.4MB)
테스트 6 〉	통과 (0.29ms, 51.7MB)
테스트 7 〉	통과 (1.50ms, 52.5MB)
테스트 8 〉	통과 (1.09ms, 52.6MB)
테스트 9 〉	통과 (1.44ms, 51.5MB)
테스트 10 〉	통과 (6.98ms, 53.2MB)
테스트 11 〉	통과 (1.49ms, 52.9MB)
테스트 12 〉	통과 (0.39ms, 53.5MB)
테스트 13 〉	통과 (0.46ms, 52.1MB)
테스트 14 〉	통과 (2.25ms, 52.3MB)
테스트 15 〉	통과 (0.38ms, 53.1MB)
테스트 16 〉	통과 (0.26ms, 52MB)
테스트 17 〉	통과 (4.02ms, 53.5MB)
테스트 18 〉	통과 (5.48ms, 53.4MB)
테스트 19 〉	통과 (5.36ms, 53.3MB)
테스트 20 〉	통과 (7.58ms, 53MB)
테스트 21 〉	통과 (8.00ms, 53MB)
테스트 22 〉	통과 (6.84ms, 53.1MB)
테스트 23 〉	통과 (6.78ms, 52.9MB)
테스트 24 〉	통과 (7.21ms, 52.4MB)
테스트 25 〉	통과 (9.55ms, 54.4MB)
테스트 26 〉	통과 (10.56ms, 53.4MB)
테스트 27 〉	통과 (7.33ms, 53.7MB)
테스트 28 〉	통과 (0.22ms, 53.1MB)
````