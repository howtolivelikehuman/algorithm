## 7-17 Longest Increasing Sequence (LIS)

| 문제ID | 시간제한 | 메모리제한 | 제출횟수 | 정답 횟수(비율) |
| :----: | :------: | :--------: | :------: | :-------------: |
|  LIS   |  2000ms  |  65536KB   |  14154   |    4067(28%)    |

#### 문제

어떤 정수 수열에서 0개 이상의 숫자를 지우면 이 수열의 부분 수열 (subsequence) 를 얻을 수 있다. 예를 들어 `10 7 4 9` 의 부분 수열에는 `7 4 9`, `10 4`, `10 9` 등이 있다. 단, `10 4 7` 은 원래 수열의 순서와 다르므로 `10 7 4 9` 의 부분 수열이 아니다.

어떤 부분 수열이 **순증가**할 때 이 부분 수열을 증가 부분 수열 (increasing subsequence) 라고 한다. 주어진 수열의 증가 부분 수열 중 가장 긴 것의 길이를 계산하는 프로그램을 작성하라. 어떤 수열의 각 수가 이전의 수보다 클 때, 이 수열을 순증가 한다고 한다.



#### 입력

입력의 첫 줄에는 테스트 케이스의 수 C (<= 50) 가 주어진다. 각 테스트 케이스의 첫 줄에는 수열에 포함된 원소의 수 N (<= 500) 이 주어진다. 그 다음 줄에 수열이 N개의 정수가 주어진다. 각 정수는 1 이상 100,000 이하의 자연수이다.



#### 출력

각 테스트케이스마다 한 줄씩, 주어진 수열의 가장 긴 증가 부분 수열의 길이를 출력한다.



#### 풀이

스택에 숫자를 오름차순으로 넣는다는 느낌으로 진행했다.

일단 가장 작은 수를 넣어주고 (-1) 숫자들을 하나씩 넣는다. 

왜냐면 -1 안넣고 시작하면 계속 첫번째 숫자에서 바꾸기만 해서 변화가 없음

만약 마지막 숫자보다 크면 (제일 큰 수임) 바로 리스트에 추가해주고,

마지막 숫자보다 작거나 같을때엔 리스트에서 이 숫자가 들어갈만한 자리에 (오름차순을 유지하는) 교환해줬다.

이렇게 하면 안에 숫자들이 최대한 작은 수로 유지되면서, 교환했기 때문에 최대값은 바뀌지 않는다. -> 다른 경우의 수를 더 볼 수 있음.

그리고 -1 넣고 시작해서 그거 빼줘야 한다.

> ex ) 9 1 3 7 6 9 5 6 8
>
> -> 1 3 7 (보단 1 3 6이 좋음) -> 1 3 6
>
> -> 1 3 6 9 에서, 1 3 5 9로 바꿔도 현재 최대값에 변화는 x
>
> 5를 살려둠으로써, 1 3 5를 갔을때 5보다 크고 9보다 작은 다른 배열의 경우의 수를 고려할 수 있게 됨
>
>  (어차피 최대값이 증가하려면, 9보다 큰 애 나오거나, 5부터 시작해서 현재 최대값까지 거리보다 더 길게 나와야함)
>
> 1 3 6 9 보다 1 3 5가 좋은 선택이려면, 1 3 5 6 7 이렇게 5부터 시작해서 3개가 더 추가되어야 함.
>
> -> 1 3 5 9 에서 6이 등장해서 -> 1 3 5 6로 바꿔줌. (실제로 1 3 ()() 5 6)
>
> -> 1 3 5 6이 되면서 8 추가 가능 -> 1 3 5 6 8

근데 막상 리스트에는 순서가 뒤죽박죽이 되어서 실제 순열이랑은 다르다.

#### 코드

```java
public class Main {
    public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    static int n, ans;
    static int[] numbers;
    static ArrayList<Integer> sub;
    
    public static void main(String[] args)throws Exception {
    		int C = Integer.parseInt(br.readLine());
    		String[] s;
    		for(int i =0; i<C; i++) {	
    			n = Integer.parseInt(br.readLine()); 
    			ans = 0;
    			numbers = new int[n];
    			sub = new ArrayList<Integer>();	
    			s = br.readLine().split(" ");	
    			for(int j =0; j<n; j++) {
    				numbers[j] = Integer.parseInt(s[j]);
    			}
    			findsub();
    			sb.append(ans).append("\n");
    		}
    		System.out.println(sb.toString());
    }
    
    public static void findsub() {
    	//일단 제일 작은 수 박아놓기.
    	sub.add(-1);
    	
    	for(int i=0; i<n; i++) {
    		//맨 끝에 수보다 크면 아예 추가
    		if(numbers[i] > sub.get(sub.size()-1)) {
    			sub.add(numbers[i]);
    		}
    		else {
    			//얘가 자리 바꾸고 들어갈 곳 (얘가 큰 애 바로 앞에 애랑 바꾸기)
    			for(int j=sub.size()-1; j>=0; --j) {
    				if(sub.get(j) < numbers[i]) {
    					sub.set(j+1, numbers[i]);
    					break;
    				}
    			}
    		}
    		System.out.println(sub);
    	} 	
    	//첨에 -1 추가해준거 빼기
    	ans = sub.size()-1;
    } 
}
```

#### 결과

| #      | 문제 | 언어 | 길이  | 결과     | 수행시간 | 제출시간 |
| ------ | ---- | ---- | ----- | -------- | -------- | -------- |
| 666846 | LIS  | JAVA | 1.3KB | **정답** | 200ms    | 2분 전   |

#### 복기

dp로 푸는 방법은 이러하다고 한다.

subsequences배열에 LIS의 길이를 저장해놓는다. 

subsequences[i] = i번째가 마지막인 순증가 최대길이라 할때

1부터 n까지 for문을 돌리는데, 이때 이전 숫자들을 매번 확인하면서 현재 길이가 최대가 될 수 있도록 한다.

뒤에

```java
public class Main {
  public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    static int n, ans;
    static int[] numbers;
    static int[] subsequences;
    
    public static void main(String[] args)throws Exception {
    	int C = Integer.parseInt(br.readLine());
    	String[] s;
    	
    	for(int i =0; i<C; i++) {
    		n = Integer.parseInt(br.readLine()); 
    		ans = 0;
    		numbers = new int[n];
    		//시작하는 숫자별 순증가 길이
    		subsequences = new int [n];    		
    		s = br.readLine().split(" ");
    		for(int j =0; j<n; j++) {
    			numbers[j] = Integer.parseInt(s[j]);
    		}
    		findsub();
    		sb.append(ans).append("\n");
    	}
    	System.out.println(sb.toString());
    }    
    public static void findsub() {
    	for(int i=0; i<n; i++) {
    		subsequences[i] = 1;
    		for(int j=0; j<i; j++) {
    			//앞에 숫자보다 크고(추가 가능)
    			if(numbers[i] > numbers[j]) {
    				//앞에 숫자들이 가진 배열 중 가장 길게 유지
    				if(subsequences[j]+1 > subsequences[i]) {
    					subsequences[i] = subsequences[i]+1;
    				}
    			}
    		}
    		ans = Math.max(subsequences[i], ans);
    	}
    }
}
```

시간복잡도는 동적으로 2차원 배열만큼 재귀로 채워감으로 $O(n^2)$ 이다.

동일한 문제 : 백준 1965번

![image-20200408223221969](C:\Users\newkid\AppData\Roaming\Typora\typora-user-images\image-20200408223221969.png)