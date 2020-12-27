## 7-16 삼각형 위의 최대 경로(TRIANGLE)

|  문제ID  | 시간제한 | 메모리제한 | 제출횟수 | 정답 횟수(비율) |
| :------: | :------: | :--------: | :------: | :-------------: |
| TRIANGLE |  5000ms  |  65536KB   |   6763   |    3495(51%)    |

#### 문제

```
6
1  2
3  7  4
9  4  1  7
2  7  5  9  4
```

위 형태와 같이 삼각형 모양으로 배치된 자연수들이 있습니다. 맨 위의 숫자에서 시작해, 한 번에 한 칸씩 아래로 내려가 맨 아래 줄로 내려가는 경로를 만들려고 합니다. 경로는 아래 줄로 내려갈 때마다 바로 아래 숫자, 혹은 오른쪽 아래 숫자로 내려갈 수 있습니다. 이 때 모든 경로 중 포함된 숫자의 최대 합을 찾는 프로그램을 작성하세요.

#### 입력

입력의 첫 줄에는 테스트 케이스의 수 C(C <= 50)가 주어집니다. 각 테스트 케이스의 첫 줄에는 삼각형의 크기 n(2 <= n <= 100)이 주어지고, 그 후 n줄에는 각 1개~n개의 숫자로 삼각형 각 가로줄에 있는 숫자가 왼쪽부터 주어집니다. 각 숫자는 1 이상 100000 이하의 자연수입니다.

#### 출력

각 테스트 케이스마다 한 줄에 최대 경로의 숫자 합을 출력합니다.

#### 풀이

동적 계획법으로, 2차원 배열 PATH에 지금까지의 경로를 저장한 뒤, 현재와 크기 비교를 한다. 그리고 다음 경로를 재귀 호출함으로 간단하게 할 수 있다.

이때, 현재 크기와 저장되어있던 경로를 비교했을때, 현재 크기가 더 작으면 더이상 재귀호출을 할 필요가 없다.(이미 더 큰 경로가 진행중) 

이렇게 복잡도를 줄여줘야 한다.

#### 코드

```java
public class Main {
    public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    static int n, ans;
    static int[][] path;
    static int[][] triangle;
    
    public static void main(String[] args)throws Exception {
    	String s;
    	String str[];
    	int C = Integer.parseInt(br.readLine());
    	
    	
    	for(int i =0; i<C; i++) {
    		n = Integer.parseInt(br.readLine());  
    		ans = 0;
    		path = new int[n][n];
    		triangle = new int[n][n]; 
    		
    		for(int y = 0; y<n; y++) {	
    			s = br.readLine().trim().replace("  ", " ");
    			str = s.split(" ");
    			
    			for(int x=0; x<=y; x++) {
    				triangle[y][x] = Integer.parseInt(str[x]);
    				path[y][x] = 0;
    			}
    			for(int x=y+1; x<n; x++) {
    				triangle[y][x] = 0;
    				path[y][x] = 0;
    			}
    		}
    		findPath(0, 0, 0);
    		sb.append(ans).append("\n");
    	}
    	System.out.println(sb.toString());
    }
    
    public static void findPath(int y, int x, int temp) {
    	int now = temp + triangle[y][x];
        //만약 더 크기가 큰 경로면
    	if(now > path[y][x]) {
    		path[y][x] = now;
    	}
        //더 크기가 작은 경로면 더 해볼필요도 없음.
    	else {
    		return;
    	}
    	
    	//끝까지 도달했으면
    	if(y == n-1) {
    		ans = Math.max(ans,now);
    		return;
    	}
    	findPath(y+1, x, now);
    	if(x <= y) {
    		findPath(y+1, x+1, now);
    	} 	
    }
}
```

#### 결과

| #            | 문제         | 언어 | 길이  | 결과     | 수행시간 | 제출시간 |
| ------------ | ------------ | ---- | ----- | -------- | -------- | -------- |
| 666694665816 | TRIANGLEPATH | JAVA | 1.4KB | **정답** | 604ms    | 31분 전  |

#### 복기

사실 예제 입력받는게 좀 극혐이었다.

보통은 공백을 하나만 하는데 예제에는 2개씩 되어있을 때 도 있어서 한참 걸렸다.

시간복잡도는 동적으로 2차원 배열만큼 재귀로 채워감으로 $O(n^2)$ 이다.

