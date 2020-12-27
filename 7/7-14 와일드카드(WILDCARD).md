## 7-14 와일드카드(WILDCARD)

|  문제ID  | 시간제한 | 메모리제한 | 제출횟수 | 정답 횟수(비율) |
| :------: | :------: | :--------: | -------- | :-------------: |
| WILDCARD |  2000ms  |  65536KB   | 9887     |    3048(30%)    |

#### 문제

와일드카드는 다양한 운영체제에서 파일 이름의 일부만으로 파일 이름을 지정하는 방법이다. 와일드카드 문자열은 일반적인 파일명과 같지만, `*` 나 `?` 와 같은 특수 문자를 포함한다.

와일드카드 문자열을 앞에서 한 글자씩 파일명과 비교해서, 모든 글자가 일치했을 때 해당 와일드카드 문자열이 파일명과 매치된다고 하자. 단, 와일드카드 문자열에 포함된 `?` 는 어떤 글자와 비교해도 일치한다고 가정하며, `*` 는 0 글자 이상의 어떤 문자열에도 일치한다고 본다.

예를 들어 와일드 카드 `he?p` 는 파일명 `help` 에도, `heap` 에도 매치되지만, `helpp` 에는 매치되지 않는다. 와일드 카드 `*p*` 는 파일명 `help` 에도, `papa` 에도 매치되지만, `hello` 에는 매치되지 않는다.

와일드카드 문자열과 함께 파일명의 집합이 주어질 때, 그 중 매치되는 파일명들을 찾아내는 프로그램을 작성하시오.

#### 입력

입력의 첫 줄에는 테스트 케이스의 수 C (1 <= C <= 10) 가 주어진다. 각 테스트 케이스의 첫 줄에는 와일드카드 문자열 W 가 주어지며, 그 다음 줄에는 파일명의 수 N (1 <= N <= 50) 이 주어진다. 그 후 N 줄에 하나씩 각 파일명이 주어진다. 파일명은 공백 없이 알파벳 대소문자와 숫자만으로 이루어져 있으며, 와일드카드는 그 외에 * 와 ? 를 가질 수 있다. 모든 문자열의 길이는 1 이상 100 이하이다.

#### 출력

각 테스트 케이스마다 주어진 와일드카드에 매치되는 파일들의 이름을 한 줄에 하나씩 아스키 코드 순서(숫자, 대문자, 소문자 순)대로 출력한다.

#### 풀이

2차원 배열 check에 와일드카드에 w번째 자리, 파일 이름의 s번째 자리에서의 규칙 여부를 -1(아직안함), 0(아님),1(맞음) 으로 저장했다.

규칙 확인 여부는 

1. check[w] [s] != -1이 아니면 (이미 확인 했으면) 그대로 결과
2. W(와일드카드), S(파일이름)이 파일 끝까지 안 갔을때, 둘의 파일명이 같거나, '?'이면 다음문자열 자리들 재귀 호출
3. W와 S모두 파일이름 끝가지 탐색이 완료되면 1(성공), 둘중에 하나만 끝까지 완료되어버리면 0(실패) -> 둘이 문자열 길이가 다르거나, *규칙에서 수정해야함
4. '*'일때 재귀호출로 w+1과 s를 비교한게 1이거나, 문자열s 끝가지 가기 전에 w와 s+1 비교한게 1이면 1(성공)
5. 모든 경우에 해당하지 않으면 0(실패)

이렇게 해서, * 상황에서 저장되어 있는 이전 비교 데이터들을 확인해, 중복되는 연산 없이 빠르게 문제를 해결 할 수 있다.

#### 코드

```java
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    static String W;
    static String S;
    static int n;
    static int[][] check = new int [101][101];
    static ArrayList<String> ans;
    static ArrayList<String> samples;
    
    public static void main(String[] args)throws Exception {
    	 int C = Integer.parseInt(br.readLine());
    	 for(int i =0; i< C; i++) { 		 
    		 W = br.readLine();
    		 n = Integer.parseInt(br.readLine().trim());
    		 samples = new ArrayList<String>();
    		 ans = new ArrayList<String>();
    		 
    		 for(int j =0; j<n; j++) {
    			S = br.readLine().trim();
     			init();
     			if(wildcard(0, 0) == 1) ans.add(S);
    		}
    		 Collections.sort(ans);
        	 for(int k =0; k<ans.size(); k++) {
        		 System.out.println(ans.get(k));
        	 }
    	 }	
    }
    
    static int wildcard(int w, int s) {   	
    	if(check[w][s] != -1) 
    		return check[w][s];	
        
    	if(w< W.length() && s<S.length()) {
    		if(W.charAt(w) == '?' || W.charAt(w) == S.charAt(s) )
    			return check[w][s] = wildcard(w+1, s+1);
    	}    	
    	if(w == W.length()) {
    		if(s== S.length()) {
    			return check[w][s] = 1;
    		}
    		else return check[w][s] = 0;
    	}   	
    	if(W.charAt(w) == '*') {
    		if(wildcard(w+1,s) == 1 || (s<S.length() && (wildcard(w,s+1) == 1)))
    			return check[w][s] = 1;
    	}	
    	return check[w][s] = 0;
    }

    static void init() {
    	for(int i = 0; i < 101; i++) {
    		for(int j = 0; j<101; j++) {
    			check[i][j] = -1;
    		}
    	}
    }   
}
```

#### 결과

| #      | 문제     | 언어 | 길이  | 결과     | 수행시간 | 제출시간  |
| ------ | -------- | ---- | ----- | -------- | -------- | --------- |
| 665816 | WILDCARD | JAVA | 1.8KB | **정답** | 172ms    | 10시간 전 |

#### 복기

시간복잡도는 중복되는 연산들을 모두 배제하고 하므로, 결론적으로 2차원 배열을 채우기만 하면 된다. 따라서 $O(N^2)$ 이라 할 수 있다.