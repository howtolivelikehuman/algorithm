## 9-9 순위 검색 (프로그래머스 2021 KAKAO BLIND RECRUITMENT)

#### 문제

**문제 링크** : https://programmers.co.kr/learn/courses/30/lessons/72412

**문제 설명**

**[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]**

카카오는 하반기 경력 개발자 공개채용을 진행 중에 있으며 현재 지원서 접수와 코딩테스트가 종료되었습니다. 이번 채용에서 지원자는 지원서 작성 시 아래와 같이 4가지 항목을 반드시 선택하도록 하였습니다.

- 코딩테스트 참여 개발언어 항목에 cpp, java, python 중 하나를 선택해야 합니다.
- 지원 직군 항목에 backend와 frontend 중 하나를 선택해야 합니다.
- 지원 경력구분 항목에 junior와 senior 중 하나를 선택해야 합니다.
- 선호하는 소울푸드로 chicken과 pizza 중 하나를 선택해야 합니다.

인재영입팀에 근무하고 있는 `니니즈`는 코딩테스트 결과를 분석하여 채용에 참여한 개발팀들에 제공하기 위해 지원자들의 지원 조건을 선택하면 해당 조건에 맞는 지원자가 몇 명인 지 쉽게 알 수 있는 도구를 만들고 있습니다.
예를 들어, 개발팀에서 궁금해하는 문의사항은 다음과 같은 형태가 될 수 있습니다.
`코딩테스트에 java로 참여했으며, backend 직군을 선택했고, junior 경력이면서, 소울푸드로 pizza를 선택한 사람 중 코딩테스트 점수를 50점 이상 받은 지원자는 몇 명인가?`

물론 이 외에도 각 개발팀의 상황에 따라 아래와 같이 다양한 형태의 문의가 있을 수 있습니다.

- 코딩테스트에 python으로 참여했으며, frontend 직군을 선택했고, senior 경력이면서, 소울푸드로 chicken을 선택한 사람 중 코딩테스트 점수를 100점 이상 받은 사람은 모두 몇 명인가?
- 코딩테스트에 cpp로 참여했으며, senior 경력이면서, 소울푸드로 pizza를 선택한 사람 중 코딩테스트 점수를 100점 이상 받은 사람은 모두 몇 명인가?
- backend 직군을 선택했고, senior 경력이면서 코딩테스트 점수를 200점 이상 받은 사람은 모두 몇 명인가?
- 소울푸드로 chicken을 선택한 사람 중 코딩테스트 점수를 250점 이상 받은 사람은 모두 몇 명인가?
- 코딩테스트 점수를 150점 이상 받은 사람은 모두 몇 명인가?

즉, 개발팀에서 궁금해하는 내용은 다음과 같은 형태를 갖습니다.

```
* [조건]을 만족하는 사람 중 코딩테스트 점수를 X점 이상 받은 사람은 모두 몇 명인가?
```

**문제**

지원자가 지원서에 입력한 4가지의 정보와 획득한 코딩테스트 점수를 하나의 문자열로 구성한 값의 배열 info, 개발팀이 궁금해하는 문의조건이 문자열 형태로 담긴 배열 query가 매개변수로 주어질 때,
각 문의조건에 해당하는 사람들의 숫자를 순서대로 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

**제한사항**

info 배열의 크기는 1 이상 50,000 이하입니다.

info 배열 각 원소의 값은 지원자가 지원서에 입력한 4가지 값과 코딩테스트 점수를 합친`개발언어 직군 경력 소울푸드 점수`형식입니다.

- 개발언어는 cpp, java, python 중 하나입니다.
- 직군은 backend, frontend 중 하나입니다.
- 경력은 junior, senior 중 하나입니다.
- 소울푸드는 chicken, pizza 중 하나입니다.
- 점수는 코딩테스트 점수를 의미하며, 1 이상 100,000 이하인 자연수입니다.
- 각 단어는 공백문자(스페이스 바) 하나로 구분되어 있습니다.

query 배열의 크기는 1 이상 100,000 이하입니다.

query의 각 문자열은`[조건] X`형식입니다.

- [조건]은 개발언어 and 직군 and 경력 and 소울푸드 형식의 문자열입니다.
- 언어는 cpp, java, python, - 중 하나입니다.
- 직군은 backend, frontend, - 중 하나입니다.
- 경력은 junior, senior, - 중 하나입니다.
- 소울푸드는 chicken, pizza, - 중 하나입니다.
- '-' 표시는 해당 조건을 고려하지 않겠다는 의미입니다.
- X는 코딩테스트 점수를 의미하며 조건을 만족하는 사람 중 X점 이상 받은 사람은 모두 몇 명인 지를 의미합니다.
- 각 단어는 공백문자(스페이스 바) 하나로 구분되어 있습니다.
- 예를 들면, cpp and - and senior and pizza 500은 cpp로 코딩테스트를 봤으며, 경력은 senior 이면서 소울푸드로 pizza를 선택한 지원자 중 코딩테스트 점수를 500점 이상 받은 사람은 모두 몇 명인가?를 의미합니다.

------



#### 풀이

처음에는 당연히 완전탐색으로 시도하였고, 효율성에서 통과하지 못했다.

info가 5만, query가 10만이라 info의 모든 키를 파싱에서 저장하고, 이에 대해 일일히 탐색을 하는 방법은 50000 * 100000 * 5번 해야하기 때문에 시간부담이 크다고 생각했다.



해시맵을 사용한 키를 활용하려고 시도하다 질문하기를 통해 방법을 알 수 있었다.

우선 모든 키의 경우의 수 (4x 3 x 3 x 3 = 36)의 해시맵과, 값들을 저장할 ArrayList를 해시맵으로 만들고,

info에 대해 키와 값을 삽입하는데,  "-"의 경우도 모두 고려하여 총 16가지의 키에 대해 값을 삽입한다.



이후 query마다 ArrayList를 얻어 범위 안에 속한 값들을 얻어내면 되는데,

여기서 그냥 순차탐색으로 하면 안되고 이진탐색으로 해야 시간안에 해결할 수 있었다.



이진탐색은 JAVA의 `collections.binarySearch`를 사용했는데 그 규칙은 다음과 같다.

1. List가 정렬되어 있어야한다.
2. 일치하는 항목이 있으면 그 index를 return한다.
3. 일치하는 항목이 없으면, -(삽입될 위치+1)을 return한다 (만약 모든 값이 다 기준보다 작으면 -배열의 크기이다.)

또한 이진탐색의 특성상, 배열에 중복이 있을 경우 홀수 짝수에 따라 항상 가장 작은 값이 탐색되지 않을 수 있다.





#### 코드

````java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;

class Solution {
	String language[] = {"cpp","java","python","-"};
	String position[] = {"backend", "frontend","-"};
	String career[] = 	{"junior","senior","-"};
	String soulfood[] = {"chicken","pizza","-"};
	String joker = "-";
	
	public void setMap(HashMap<String, ArrayList<Integer>> map) {
		for(int a=0; a<language.length; a++) {
			for(int b=0; b<position.length; b++) {
				for(int c=0; c<career.length; c++) {
					for(int d=0; d<soulfood.length; d++) {
						map.put(language[a]+position[b]+career[c]+soulfood[d], new ArrayList<Integer>());
					}
				}
			}
		}
	}
	//모든 16가지 경우 전부 다
	public void inputVal(HashMap<String, ArrayList<Integer>> map, String info, int cutPoint, int value) {
		String[] token = (info.substring(0, cutPoint-1)).split(" ");
		map.get(token[0] + token[1] + token[2]+ token[3]).add(value);
		map.get(token[0] + token[1] + token[2]+ joker).add(value);
		map.get(token[0] + token[1] + joker+ token[3]).add(value);
		map.get(token[0] + joker + token[2]+ token[3]).add(value);
		map.get(joker + token[1] + token[2]+ token[3]).add(value);
		map.get(token[0] + token[1] + joker+ joker).add(value);
		map.get(token[0] + joker + token[2]+ joker).add(value);
		map.get(joker + token[1] + token[2]+ joker).add(value);
		map.get(token[0] + joker + joker+ token[3]).add(value);
		map.get(joker + token[1] + joker+ token[3]).add(value);
		map.get(joker + joker + token[2]+ token[3]).add(value);
		map.get(token[0] +joker + joker+ joker).add(value);
		map.get(joker + joker + token[2]+ joker).add(value);
		map.get(joker + token[1] + joker+ joker).add(value);
		map.get(joker + joker + joker+ token[3]).add(value);
		map.get(joker + joker + joker+ joker).add(value);
	}
	
	
    public int[] solution(String[] info, String[] query) {
    	
    	int[] answer = new int[query.length];
    	HashMap<String, ArrayList<Integer>> infoMap = new HashMap<>(); 
    	setMap(infoMap);
    	
    	for(int i = 0; i<info.length; i++) {
    		int cutPoint = info[i].lastIndexOf(" ")+1;
    		int value = Integer.parseInt(info[i].substring(cutPoint));
    		inputVal(infoMap, info[i], cutPoint, value);
        }
    	
    	for (Map.Entry<String, ArrayList<Integer>> entry : infoMap.entrySet()) {
			Collections.sort(entry.getValue());
		}
    	
    	for (int i=0; i < query.length; i++) {
    		int cutPoint = query[i].lastIndexOf(" ")+1;
    		int value = Integer.parseInt(query[i].substring(cutPoint));
    		String key = query[i].substring(0,cutPoint-1).replaceAll(" and ", "");
    		
    		ArrayList<Integer> output = infoMap.get(key);
    		int idx = Collections.binarySearch(output, value);
    		if(idx >= 0) {
                for(int a=idx-1; a>=0; a--) {
    				if(output.get(idx) - output.get(a) > 0) break;
    				else idx = a;
    			}
    			answer[i] = output.size()-idx;
    		}else {
    			answer[i] = output.size()+idx+1;
    		}
    		//System.out.println(answer[i]);
    	}
        return answer;
    }
}
````



#### 결과

![img](https://blog.kakaocdn.net/dn/cjwyus/btqXLf7J8NC/oZ2DkpjJb6s167kEjdQUJ1/img.png)