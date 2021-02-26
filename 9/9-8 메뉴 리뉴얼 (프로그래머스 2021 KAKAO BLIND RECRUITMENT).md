## 9-8 메뉴 리뉴얼 (프로그래머스 2021 KAKAO BLIND RECRUITMENT)

#### 문제

**문제 링크** : https://programmers.co.kr/learn/courses/30/lessons/72411

**문제 설명**

레스토랑을 운영하던 `스카피`는 코로나19로 인한 불경기를 극복하고자 메뉴를 새로 구성하려고 고민하고 있습니다.
기존에는 단품으로만 제공하던 메뉴를 조합해서 코스요리 형태로 재구성해서 새로운 메뉴를 제공하기로 결정했습니다. 어떤 단품메뉴들을 조합해서 코스요리 메뉴로 구성하면 좋을 지 고민하던 스카피는 이전에 각 손님들이 주문할 때 가장 많이 함께 주문한 단품메뉴들을 코스요리 메뉴로 구성하기로 했습니다.
단, 코스요리 메뉴는 최소 2가지 이상의 단품메뉴로 구성하려고 합니다. 또한, 최소 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴 후보에 포함하기로 했습니다.

예를 들어, 손님 6명이 주문한 단품메뉴들의 조합이 다음과 같다면,
(각 손님은 단품메뉴를 2개 이상 주문해야 하며, 각 단품메뉴는 A ~ Z의 알파벳 대문자로 표기합니다.)

| 손님 번호 | 주문한 단품메뉴 조합 |
| --------- | -------------------- |
| 1번 손님  | A, B, C, F, G        |
| 2번 손님  | A, C                 |
| 3번 손님  | C, D, E              |
| 4번 손님  | A, C, D, E           |
| 5번 손님  | B, C, F, G           |
| 6번 손님  | A, C, D, E, H        |

가장 많이 함께 주문된 단품메뉴 조합에 따라 스카피가 만들게 될 코스요리 메뉴 구성 후보는 다음과 같습니다.

| 코스 종류     | 메뉴 구성  | 설명                                                 |
| ------------- | ---------- | ---------------------------------------------------- |
| 요리 2개 코스 | A, C       | 1번, 2번, 4번, 6번 손님으로부터 총 4번 주문됐습니다. |
| 요리 3개 코스 | C, D, E    | 3번, 4번, 6번 손님으로부터 총 3번 주문됐습니다.      |
| 요리 4개 코스 | B, C, F, G | 1번, 5번 손님으로부터 총 2번 주문됐습니다.           |
| 요리 4개 코스 | A, C, D, E | 4번, 6번 손님으로부터 총 2번 주문됐습니다.           |

**문제**

각 손님들이 주문한 단품메뉴들이 문자열 형식으로 담긴 배열 orders, 스카피가 `추가하고 싶어하는` 코스요리를 구성하는 단품메뉴들의 갯수가 담긴 배열 course가 매개변수로 주어질 때, 스카피가 새로 추가하게 될 코스요리의 메뉴 구성을 문자열 형태로 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

**제한사항**

orders 배열의 크기는 2 이상 20 이하입니다.

orders 배열의 각 원소는 크기가 2 이상 10 이하인 문자열입니다.

- 각 문자열은 알파벳 대문자로만 이루어져 있습니다.
- 각 문자열에는 같은 알파벳이 중복해서 들어있지 않습니다.

course 배열의 크기는 1 이상 10 이하입니다.

- course 배열의 각 원소는 2 이상 10 이하인 자연수가 `오름차순`으로 정렬되어 있습니다.
- course 배열에는 같은 값이 중복해서 들어있지 않습니다.

정답은 각 코스요리 메뉴의 구성을 문자열 형식으로 배열에 담아 사전 순으로 정렬해서 return 해주세요.

- 배열의 각 원소에 저장된 문자열 또한 알파벳 `오름차순`으로 정렬되어야 합니다.
- 만약 가장 많이 함께 주문된 메뉴 구성이 여러 개라면, 모두 배열에 담아 return 하면 됩니다.
- orders와 course 매개변수는 return 하는 배열의 길이가 1 이상이 되도록 주어집니다.

------



#### 풀이

마땅히 풀이가 생각나지 않아서 진짜 정말정말정말 지저분하게 푼 것 같다.

우선 각각의 손님의 주문내역을 배열로 표현하고 싶었는데,

우선 알파벳 26자에 대해 다 해보기는 싫으므로, 입력으로 들어오는 문자열의 종류만 하고 싶었다.

그래서 HashMap을 사용하여 매핑했다. (문자 -> 숫자(index)) 이때 역으로 돌릴 수도 있으므로 

반대 버전도 똑같이 만들었다.

````java
public int getRange(final String[] orders, HashMap<Character,Integer> indexmapper, HashMap<Integer, Character> reversemapper){
    int index = 0;
    for(int i=0; i<orders.length; i++) {
        char[] list = orders[i].toCharArray();
        for(char a : list) {
            if(!indexmapper.containsKey(a)) {
                indexmapper.put(a, index);
                reversemapper.put(index++, a);
            }
        }
    }
    //전체 원소 개수
    return index;
}
````



그리고 이제 각각의 주문을 배열로 표현하였는데, 이때 배열의 맨 마지막은 각 주문의 길이로 하였다.

````java
public void setMap(final String[] orders, int[][] roadMap, HashMap<Character,Integer> indexmapper) {
    for(int i=0; i<orders.length; i++) {
        char[] list = orders[i].toCharArray();
        for(char a : list) {
            int x = indexmapper.get((Character)a);
            roadMap[i][x]++;
        }
        //맨 마지막 원소는 전체 주문의 길이
        roadMap[i][roadMap[i].length-1] = list.length;
    }
}
````



그럼 이제 배열이 이런 형태로 나온다

````
1 1 1 1 1 0 0 0 5 
1 1 0 0 0 0 0 0 2 
0 0 1 1 0 0 0 0 2 
1 0 0 1 1 0 0 0 3 
0 0 0 0 0 1 1 1 3 
0 0 0 0 0 1 1 1 3 
1 0 1 1 0 0 0 0 3 
````



이제 다음으로는 각각의 세트매뉴 수 (2,3,4..)만큼 조합을 만들어서 풀려고 하였다.

일단 현재 배열 형태이므로, 배열의 주소를 조합으로 만들어서 

그 주소를 검사하고 -> 이제 이 주소에 따른 가장 많이 포함된 배열이 나오고 -> 다시 문자열로 바꾸기

이 과정을 통해 정답을 얻어내었다.



````java
ArrayList<int[]> answerInt;
ArrayList<String> answer;
public void findMax(int[] course, int[][] roadMap) {
    //10, 9, 8 ... 
    for(int i=course.length-1; i>=0; i--) {
        int[] arr = new int[course[i]];
        answerInt = new ArrayList<>();
        answerCount = 2; //2개 이하는 안되게

        Combination(answerset, 0, answerset.size(), course[i], 0, arr);
        //문자열로 변환
        setString();
    }
}
````



````java
public void Combination(ArrayList<Integer> set, int index, int n, int k, int t, int[] newcomb) {
    if(k ==0 ) {
        //얼마나 포함되었는지 찾아서
        int count = GetNumb(newcomb);
        if(count >= answerCount) {
            if(count > answerCount) { //최대 이상이면, 저장된거 다 비우고 새로 담기
                answerInt.clear();
                answerCount = count;
            }//아니면 그냥 저장된 거에 추가
            answerInt.add(newcomb.clone());
        }
        return;
    }
    if(t == n) return;
    newcomb[index] = set.get(t);
    Combination(set, index+1, n, k-1, t+1, newcomb);
    Combination(set, index, n, k, t+1, newcomb);
}
````

이 과정을 모든 course에 대해 수행하고, 정렬한 뒤 답을 출력하면 된다.

전체 코드는 다음과 같다.

#### 코드

````java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;

class Solution {
	
	HashMap<Character,Integer> indexmapper;
	HashMap<Integer, Character> reversemapper;
	ArrayList<int[]> answerInt;
	ArrayList<String> answer;
	ArrayList<Integer> answerset;
	
	int answerCount;
	int roadMap[][];
	int charsize;
	
    public String[] solution(String[] orders, int[] course) {
    	
    	//map for each char
    	indexmapper = new HashMap<>();
    	reversemapper = new HashMap<>();
    	
    	charsize = getRange(orders, indexmapper, reversemapper);
    	roadMap = new int[orders.length][charsize+1];
    	setMap(orders, roadMap, indexmapper);
    	
    	answerset = new ArrayList<>(indexmapper.values());
    	answer = new ArrayList<>();
    	
    	//get the answer
    	findMax(course, roadMap);
    	
    	Collections.sort(answer);
    	
    	return answer.toArray(new String[answer.size()]);
    }
    
    
    public int getRange(final String[] orders, HashMap<Character,Integer> indexmapper, HashMap<Integer, Character> reversemapper){
    	int index = 0;
    	for(int i=0; i<orders.length; i++) {
    		
    		char[] list = orders[i].toCharArray();
    		    		
    		for(char a : list) {
    			
    			if(!indexmapper.containsKey(a)) {
    				indexmapper.put(a, index);
    				reversemapper.put(index++, a);
    			}
    		}
    		
    	}
    	return index;
    }
    
    public void setMap(final String[] orders, int[][] roadMap, HashMap<Character,Integer> indexmapper) {
    	for(int i=0; i<orders.length; i++) {
    		
    		char[] list = orders[i].toCharArray();
    		
    		for(char a : list) {
    			int x = indexmapper.get((Character)a);
    			roadMap[i][x]++;
    		}
    		
    		roadMap[i][roadMap[i].length-1] = list.length;
    	}
    }

    public void findMax(int[] course, int[][] roadMap) {
    	
    	//10, 9, 8 ... 
    	for(int i=course.length-1; i>=0; i--) {
    		int[] arr = new int[course[i]];
        	answerInt = new ArrayList<>();
    		answerCount = 2;
    		
    		Combination(answerset, 0, answerset.size(), course[i], 0, arr);
    		setString();
    	}
    }
    
    public void setString() {
    	for(int[] list : answerInt) {
    		String s = "";
    		for(int i=0; i<list.length; i++) {
    			s = s+reversemapper.get(list[i]);
    		}
    		answer.add(s);
    	}
    }
    
    public void Combination(ArrayList<Integer> set, int index, int n, int k, int t, int[] newcomb) {
    	if(k ==0 ) {
    		int count = GetNumb(newcomb);
    		if(count >= answerCount) {
    			if(count > answerCount) {
    				answerInt.clear();
        			answerCount = count;
    			}
    			answerInt.add(newcomb.clone());
    		}
    		return;
    	}
    	if(t == n) return;
    	newcomb[index] = set.get(t);
    	Combination(set, index+1, n, k-1, t+1, newcomb);
    	Combination(set, index, n, k, t+1, newcomb);
    }
    
    
    public int GetNumb(int[] comb) {
    	int count = 0;
    	
    	for(int i=0; i<roadMap.length; i++) {
    		if(roadMap[i][charsize] < comb.length) {
    			continue;
    		}
    		boolean success = true;
    		
    		for(int i2=0; i2<comb.length; i2++) {
    			if(roadMap[i][comb[i2]] != 1) {
    				success = false;
    				break;
    			}
    		}
    		if(success) {
    			count++;
    		}
    	}
    	return count;
    }    
}
````



#### 결과

![1](C:\Users\newkid\Desktop\1.png)