## 8-16 괄호 변환 (프로그래머스) [JAVA] 2020 KAKAO BLIND RECRUITMENT

#### 문제

그녀는 동적으로 게임 시간을 늘려서 난이도를 조절하기로 했다. 역시 슈퍼 개발자라 대부분의 로직은 쉽게 구현했지만, 실패율을 구하는 부분에서 위기에 빠지고 말았다. 오렐리를 위해 실패율을 구하는 코드를 완성하라.

- 실패율은 다음과 같이 정의한다.
  - 스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수

전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 solution 함수를 완성하라.

##### 제한사항

- 스테이지의 개수 N은 `1` 이상 `500` 이하의 자연수이다.
- stages의 길이는 `1` 이상 `200,000` 이하이다.
- stages에는 `1` 이상 `N + 1` 이하의 자연수가 담겨있다.
  - 각 자연수는 사용자가 현재 도전 중인 스테이지의 번호를 나타낸다.
  - 단, `N + 1` 은 마지막 스테이지(N 번째 스테이지) 까지 클리어 한 사용자를 나타낸다.
- 만약 실패율이 같은 스테이지가 있다면 작은 번호의 스테이지가 먼저 오도록 하면 된다.
- 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 `0` 으로 정의한다.

##### 입출력 예

| N    | stages                   | result      |
| ---- | ------------------------ | ----------- |
| 5    | [2, 1, 2, 6, 2, 4, 3, 3] | [3,4,2,1,5] |
| 4    | [4,4,4,4,4]              | [4,1,2,3]   |

#### 풀이

생각보다 까다로운 문제였다. 머리를 많이 굴리기 싫어서 편하게 풀었다.

일단 전체 stage에 대한 기록들을 배열의 주소를 index로 담은다음에 -> 누적 합을하는 배열을 다시 만들었다.

그리고 다시 얘네를 나눠서 실패율을 구했다.

그리고 index를 보전하면서 내림차순 정렬을 해야하는데..

인터넷에 HashMap 내림차순 정렬하는법을 검색해서 참고하였다.

key로 index (stage), value로 실패율을 넣은 HashMap을 선언하고

ArrayList의 sort함수에 value를 2개씩 비교하게 Comparator 를 새로 적용해서 value값으로 키가 정렬되게 했다.



#### 코드

````java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.List;

class Solution {
    public int[] solution(int N, int[] stages) {
    	float[] test = new float[N+2];
    	int[] test2 = new int[N+2];
    	
    	
    	for(int i=0; i<stages.length; i++) {
    		test[stages[i]]++;
    	}
    	test2[N] = (int)test[N+1]+(int)test[N];
    	
    	for(int i=N; i>0; i--) {
    		test2[i-1] = (int)test[i-1] + test2[i];
    		//System.out.println(i + " " +test[i] + " " + test2[i]);
    		if(test2[i] == 0) {
    			test[i] = 0;
    		}
    		else {
    			test[i] = test[i]/ test2[i];
    		}
    	}
    	
        
    	HashMap<Integer, Float> a = new LinkedHashMap<>();
    	for(int i=1; i<N+1; i++) {
    		a.put(i, test[i]);
    	}
        //HashMap 내림차순
    	List<Integer> a2 = new ArrayList<>(a.keySet());
    	Collections.sort(a2, (o1, o2) -> (a.get(o2).compareTo(a.get(o1))));
    	
    	int[] answer= new int[N];
    	int i=0;
    	for(Integer key : a2) {
    		answer[i++] = key;
    		//System.out.println(answer[i-1]);
    	}
    	 return answer;
    }
   
}
````

#### 결과

````
테스트 1 〉	통과 (1.10ms, 52.4MB)
테스트 2 〉	통과 (0.96ms, 53.5MB)
테스트 3 〉	통과 (4.68ms, 56.9MB)
테스트 4 〉	통과 (6.66ms, 57MB)
테스트 5 〉	통과 (8.46ms, 61.4MB)
테스트 6 〉	통과 (1.55ms, 52.7MB)
테스트 7 〉	통과 (2.06ms, 53.2MB)
테스트 8 〉	통과 (5.19ms, 56.8MB)
테스트 9 〉	통과 (12.59ms, 64.5MB)
테스트 10 〉	통과 (5.03ms, 55.9MB)
테스트 11 〉	통과 (10.80ms, 59.2MB)
테스트 12 〉	통과 (5.45ms, 57.3MB)
테스트 13 〉	통과 (10.73ms, 62.5MB)
테스트 14 〉	통과 (0.92ms, 51.8MB)
테스트 15 〉	통과 (3.06ms, 54.3MB)
테스트 16 〉	통과 (1.99ms, 53.3MB)
테스트 17 〉	통과 (2.91ms, 55.2MB)
테스트 18 〉	통과 (1.94ms, 53.8MB)
테스트 19 〉	통과 (1.29ms, 53.5MB)
테스트 20 〉	통과 (2.52ms, 54.8MB)
테스트 21 〉	통과 (3.76ms, 55.3MB)
테스트 22 〉	통과 (10.11ms, 62.2MB)
테스트 23 〉	통과 (9.07ms, 59.1MB)
테스트 24 〉	통과 (4.78ms, 59.3MB)
테스트 25 〉	통과 (1.07ms, 52.4MB)
테스트 26 〉	통과 (0.95ms, 51.7MB)
테스트 27 〉	통과 (1.12ms, 52.4MB)
````