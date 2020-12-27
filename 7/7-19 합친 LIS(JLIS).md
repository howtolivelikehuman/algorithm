## 7-19 합친 LIS(JLIS)

| 문제ID | 시간제한 | 메모리제한 | 제출횟수 | 정답 횟수(비율) |
| :----: | :------: | :--------: | :------: | :-------------: |
|  LIS   |  2000ms  |  65536KB   |   6531   |    1555(23%)    |

#### 문제

어떤 수열에서 0개 이상의 숫자를 지운 결과를 원 수열의 **부분 수열**이라고 부릅니다. 예를 들어 '4 7 6'은 '4 3 7 6 9'의 **부분 수열**입니다. 중복된 숫자가 없고 오름 차순으로 정렬되어 있는 **부분 수열**들을 가리켜 **증가 부분 수열**이라고 부르지요. 예를 들어 '3 6 9'는 앞의 수열의 **증가 부분 수열**입니다.

두 개의 정수 수열 A 와 B 에서 각각 **증가 부분 수열**을 얻은 뒤 이들을 크기 순서대로 합친 것을 **합친 증가 부분 수열**이라고 부르기로 합시다. 이 중 가장 긴 수열을 **합친 LIS**(JLIS, Joined Longest Increasing Subsequence)이라고 부릅시다. 예를 들어 '1 3 4 7 9' 은 '1 9 4' 와 '3 4 7' 의 **JLIS**입니다. '1 9' 와 '3 4 7' 을 합쳐 '1 3 4 7 9'를 얻을 수 있기 때문이지요.

A 와 B 가 주어질 때, **JLIS**의 길이를 계산하는 프로그램을 작성하세요.



#### 입력

입력의 첫 줄에는 테스트 케이스의 수 c ( 1 <= c <= 50 ) 가 주어집니다. 각 테스트 케이스의 첫 줄에는 A 와 B 의 길이 n 과 m 이 주어집니다 (1 <= n,m <= 100). 다음 줄에는 n 개의 정수로 A 의 원소들이, 그 다음 줄에는 m 개의 정수로 B 의 원소들이 주어집니다. 모든 원소들은 32비트 부호 있는 정수에 저장할 수 있습니다.



#### 출력

각 테스트 케이스마다 한 줄에, JLIS 의 길이를 출력합니다.



#### 풀이

JLIST(index A, index B) = A[index A]와 B[index B]에서 시작하는 합친 증가 부분수열의 최대 길이

작은 쪽이 앞에 온다고 할때 증가 부분수열의 다음 숫자는 A[indexA+1] 이후 or B[index B+1]이후의 순열 중 max(A[indexA], B[indexB])보다 큰 수 중 하나.

이때 A[nextA]가  다음숫자일때 합친 즈가부분수열의 최대 길이 = 1+JLIS(nextA, indexB)

JLIS(index A, index B) = max ( max(JLIS(index A, index B),	JLIS(nextA,indexB)+1) , max( JLIS(index A, index B),	JLIS(indexA,nextB) +1 ) 	)

이때 A[-1] = 최소, B[-1] = 최소 로 두고 항상 JLIS에 포함된다고 함 (마지막에 -2 해주기)

그렇게 해서 0부터 탐색 (0일때 원소 1개)



최적화 문제 동적 계획법 레시피

> 1. 모든 답을 만들어 보고 최적해의 점수를 반환하는 완전 탐색 알고리즘 설계
> 2. 전체 답의 점수를 반환하는 것이 아니라, 앞으로 남은 선택들에 해당하는 점수만 반환하도록 부분문제 바꾸기
> 3. 재귀 호출의 입력에 이전에 선택에 관련된 정보가 있다면 필요한 것만 남기고 줄이기. 가능한 한 중복문제 줄이기
> 4. 문제가 최적 부분 구조면 이전 선택 정보들 아예 없애는 것도 가능.
> 5. 입력이 배열이거나 문자열이면 적절한 변환으로 메모제이션 적용.



#### 코드

```java
public class Main {
  	public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    static Integer[][] subsets;
    static Integer[] a;
    static Integer[] b;
    
    public static void main(String[] args)throws Exception {
    	int c = Integer.parseInt(br.readLine());
    	for( ; c>0; c--) {
    		String stl[] = br.readLine().split(" ");
    		int ans = 0;
    		a = new Integer[Integer.parseInt(stl[0])];
    		b = new Integer[Integer.parseInt(stl[1])];
            //-1,-1 (부분수열을 둘다 안합칠때부터 시작하므로)
    		subsets = new Integer[a.length+1][b.length+1];
    		
    		for(int i =0; i<a.length+1; i++) {
    			for(int j=0; j<b.length+1; j++){
    				subsets[i][j] = -1;
    			}
    		}
    		
    		stl = br.readLine().split(" ");
    		for(int i =0; i<a.length; i++) {
    			a[i] = Integer.parseInt(stl[i]);
    		}
    		stl = br.readLine().split(" ");
    		for(int i =0; i<b.length; i++) {
    			b[i] = Integer.parseInt(stl[i]);
    		}
            //실제 답에서 -2 빼주기
    		ans = getMax(-1,-1) - 2;
    		sb.append(ans).append("\n");
    	}
    	System.out.println(sb.toString());
    }
    
    public static int getMax(int ai, int bi) {
    	//만약 값이 있으면 리턴
    	if(subsets[ai+1][bi+1] != -1) return subsets[ai+1][bi+1];   	
        
    	//ai가 -1이면 long 최솟값, 아니면 a[ai]
    	long am  = (ai == -1 ? Long.MIN_VALUE : (long)a[ai]);
    	long bm  = (bi == -1 ? Long.MIN_VALUE : (long)b[bi]);
        
        //그중 최대 (걔가 앞에오니깐)
    	long maxnum = Math.max(am, bm);
        
        //am, bm이 존재 (-1 즉 둘중에 하나가 공집합이어도 일단은 최소값으로 넣었음) 하므로 2개는 있다고 생각.
    	int sum = 2;
        
    	for (int i = ai+1; i < a.length; i++) {
    		if(maxnum < a[i])
    			sum = Math.max(sum, getMax(i,bi)+1);
    	}
    	for (int i = bi+1; i < b.length; i++) {
    		if(maxnum < b[i])
    			sum = Math.max(sum, getMax(ai,i)+1);
    	}
    	subsets[ai+1][bi+1] = sum;   	
    	return sum;
    }
}
```

#### 결과

| #      | 문제 | 언어 | 길이  | 결과     | 수행시간 | 제출시간 |
| ------ | ---- | ---- | ----- | -------- | -------- | -------- |
| 667457 | JLIS | JAVA | 1.8KB | **정답** | 1500ms   | 2분 전   |

#### 복기

```java
import java.util.Vector;
public class Main {
    public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
   
    public static void main(String[] args)throws Exception {
    	int c = Integer.parseInt(br.readLine());
    	for( ; c>0; c--) {
    		String stl[] = br.readLine().split(" ");
    		int ans = 0;
    		int[] a = new int[Integer.parseInt(stl[0])];
    		int[] b = new int[Integer.parseInt(stl[1])];
    		
    		stl = br.readLine().split(" ");
    		for(int i =0; i<a.length; i++) {
    			a[i] = Integer.parseInt(stl[i]);
    		}
    		stl = br.readLine().split(" ");
    		for(int i =0; i<b.length; i++) {
    			b[i] = Integer.parseInt(stl[i]);
    		}
    		
    		ans = getMax(findsub(a), findsub(b));	
    		sb.append(ans).append("\n");
    	}
    	System.out.println(sb.toString());
    }
  
    public static Vector<Integer>[] findsub(int[] numbers) {   	
    	Vector<Integer> subsequences[] = new Vector[numbers.length];
    	int[] lists = new int[numbers.length];
    	for(int i=0; i<numbers.length; i++) {
    		subsequences[i] = new Vector<Integer>();
    		subsequences[i].add(numbers[i]);
    		for(int j=0; j<i; j++) {
    			//앞에 숫자보다 크고(추가 가능)
    			if(numbers[i] > numbers[j]) {
    				//앞에 숫자들이 가진 배열 중 가장 길게 유지
    				if(subsequences[j].size()+1 > subsequences[i].size()) {
    					subsequences[i].clear();
    					subsequences[i].add(numbers[i]);
    					subsequences[i].addAll(subsequences[j]);
    					
    					lists[j] = 1;
    				}
    			}
    		}
    	}
    	for(int i=0; i< lists.length; i++) {
    		if(lists[i] == 1) {
    			subsequences[i].clear();
    		}
    	}

    	return subsequences;
    }     
    public static int getMax(Vector<Integer> a[], Vector<Integer> b[]) {
    	int ans = 0;
    	for(int ai =0; ai<a.length; ai++) {
    		if(a[ai].size() == 0) continue;
    		for(int bi =0; bi<b.length; bi++) {
    			if(b[bi].size() == 0) continue;
    			ans = Math.max( ans , isdup(a[ai],b[bi]));
    		}
    	}
    	return ans;
    }
    public static int isdup(Vector<Integer> a, Vector<Integer> b) {
    	int sum = a.size() + b.size();
    	
    	for(int ai =0; ai< a.size(); ai++) {
    		for(int bi =0; bi< b.size(); bi++) {
    			if(a.get(ai) - b.get(bi) == 0) sum--;  			
    		}
    	}
    	return sum;
    }
}
```

도합 $O(n^6)$ 의 어마무시한 코드.