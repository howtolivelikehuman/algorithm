## 8-12 위장 (프로그래머스)

#### 문제

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류 | 이름                       |
| ---- | -------------------------- |
| 얼굴 | 동그란 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠              |
| 하의 | 청바지                     |
| 겉옷 | 긴 코트                    |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.

##### 입출력 예

| clothes                                                      | return |
| ------------------------------------------------------------ | ------ |
| [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear]] | 5      |
| [[crow_mask, face], [blue_sunglasses, face], [smoky_makeup, face]] | 3      |

#### 풀이

일단 문제를 풀면서 처음 생각한 것은, 옷의 이름은 별로 의미가 없다는 점과 조합이 필요하단 점이다.

애초에 중복된 옷이 없기 때문에 단지 필요한 것은 각각의 종류별 옷의 개수! 그리고 이를 토대로 조합을 만들어서 옷을 입는 가짓 수를 생각하면 되겠다고 했다.

그렇다면 종류별 옷의 개수를 어떻게 알아낼 것인가.. 평소였다면 일일히 비교해서 배열에 넣고 어쩌고 했겠지만, 지난번 해쉬맵 문제 이후 해쉬 맵을 사용하기로 했다.

key에 옷의 종류인 2번 인덱스를 넣고, 계속 숫자를 갱신해서 최종적으로는 옷의 종류만큼의 해시맵 인자와 각 종류별 개수가 저장되게 하였다.

이제 이 숫자들로 옷을 1개만 입는 가짓수 ~ N개 입는 가짓수를 해서 조합을 짰다.

ㄱ종류 a개 , ㄴ종류 b개인 경우 -> $_{2}\mathrm{C}_{1}$ +의 모든 옷수(a + b) +$_{2}\mathrm{C}_{2}$인 경우 (ab)라고 생각했기 때문이다.

그래서 각 종류별 옷의 개수를 담은 1차원 배열을 만들고, 조합을 짠뒤 각각의 값들을 곱해서 계속 정답에 더해주는 방식으로 풀었다.

**하지만 이렇게 풀면 1번 문제에서 시간초과가 발생한다.**

총 30벌의 옷 데이터가 들어오는데, 최악의 경우에는 $_{30}\mathrm{C}_{1}$ ~$ _{30}\mathrm{C}_{30}$까지 모두 조합을 짜야 할 수도 있다. 단순히 조합의 개수만 찾는 문제라면 DP를 활용해도 되나, 이 문제에선 각각의 옷 종류별로 개수가 또 다르기 때문에 엄청 복잡해진다.

이때 수학적으로 문제를 접근해야 한다.  아까 위의 ㄱ,ㄴ종류의 옷을 보자.

가능한 가짓수는 $_{2}\mathrm{C}_{1}$(한 종류만 입기) $_{2}\mathrm{C}_{2}$ (두 종류 입기) 두 가지고 이는 a + b + ab = (a+1)(b+1) -1로 식이 정리된다.

만약에 a, b, c개의 3종류라고 생각해보면

 a+ b + c + ab+ ac+ bc + abc = a(1+b+c+bc) + b+ c+ bc = a(b+1)(c+1) + (b+1)(c+1) -1 = (a+1)(b+1)(c+1) -1

결국 우리는 위에서 옷의 종류별 개수만 파악한 다음 +1해줘서 다 곱하고 -1하면 되는 것이었다...

#### 코드

````java
class Solution {
	int answer = 1;
    public int solution(String[][] clothes) {  

        HashMap<String,Integer> map = new HashMap<String,Integer>();
        
        //사실 옷의 이름은 별로 필요가 없음 (중복이 안되므로)
        for(int i=0; i< clothes.length; i++) {  

        	if(map.containsKey(clothes[i][1])) {
        		map.put(clothes[i][1], map.get(clothes[i][1])+1);
        	}
        	else {
        		map.put(clothes[i][1], 1);
        	}
        }
        int i=0;
        
        for (String key : map.keySet()) {
        	answer = answer * (map.get(key) +1);
        } 
        return answer-1;
    }
    
}
````

````java
import java.util.HashMap;
class Solution {
	int answer = 0;
    public int solution(String[][] clothes) {  
        int variation = 0;
        HashMap<String,Integer> map = new HashMap<String,Integer>();
        
        //사실 옷의 종류는 별로 필요가 없음 (중복이 안되므로)
        for(int i=0; i< clothes.length; i++) {  

        	if(map.containsKey(clothes[i][1])) {
        		map.put(clothes[i][1], map.remove(clothes[i][1])+1);
        	}
        	else {
        		map.put(clothes[i][1], 1);
        		variation++;
        	}
        }
        
        int[] clothesCount = new int[variation];
        int[] forComb = new int[variation];
        int i=0;
        
        for (String key : map.keySet()) {
        	clothesCount[i] = map.get(key);        	
        	i++;
        }
        
        for(i=1; i<=variation; i++) {
        	Combination(clothesCount, 0, variation, i, 0, forComb, i);
        }
        return answer;
    }
    
    public void Combination(int[] arr, int index, int n, int k, int t, int[] newcomb, int length) {
    	
    	if(k == 0) {
    		int temp = 1;
    		
    		for(int i=0; i<length; i++){
    				temp = temp * newcomb[i];
    		}
    		
    		answer = answer + temp;
    		return;
    	}
    	
    	if(t == n) return;
    	
    	newcomb[index] = arr[t];
    	Combination(arr,index+1, n, k-1,t+1, newcomb, length); //선택했을 때
    	Combination(arr,index, n, k, t+1, newcomb, length); //선택 안했을 때
    }
}
````

````
테스트 1 〉	통과 (0.06ms, 69MB)
테스트 2 〉	통과 (0.09ms, 68.8MB)
테스트 3 〉	통과 (0.08ms, 70.3MB)
테스트 4 〉	통과 (0.07ms, 69MB)
테스트 5 〉	통과 (0.06ms, 68.3MB)
테스트 6 〉	통과 (0.07ms, 68.8MB)
테스트 7 〉	통과 (0.07ms, 68.3MB)
테스트 8 〉	통과 (0.08ms, 67.9MB)
테스트 9 〉	통과 (0.16ms, 69.3MB)
테스트 10 〉	통과 (0.20ms, 70MB)
테스트 11 〉	통과 (0.07ms, 68.6MB)
테스트 12 〉	통과 (0.07ms, 68.1MB)
테스트 13 〉	통과 (0.10ms, 68.6MB)
테스트 14 〉	통과 (0.06ms, 69.6MB)
테스트 15 〉	통과 (0.05ms, 68.3MB)
테스트 16 〉	통과 (0.05ms, 68.2MB)
테스트 17 〉	통과 (0.09ms, 68.7MB)
테스트 18 〉	통과 (0.08ms, 68.9MB)
테스트 19 〉	통과 (0.07ms, 68.7MB)
테스트 20 〉	통과 (0.10ms, 68.9MB)
테스트 21 〉	통과 (0.07ms, 68.6MB)
테스트 22 〉	통과 (0.05ms, 68.8MB)
테스트 23 〉	통과 (0.07ms, 68.1MB)
테스트 24 〉	통과 (0.07ms, 68.8MB)
테스트 25 〉	통과 (0.06ms, 68.1MB)
테스트 26 〉	통과 (0.10ms, 67.7MB)
테스트 27 〉	통과 (0.19ms, 69.1MB)
테스트 28 〉	통과 (0.07ms, 68.1MB)
````