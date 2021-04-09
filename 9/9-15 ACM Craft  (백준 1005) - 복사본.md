## 9-15 ACM Craft (백준 1005)

#### 문제

**문제 링크** : https://www.acmicpc.net/problem/1005

| 시간 제한 | 메모리 제한 | 제출  | 정답 | 맞은 사람 | 정답 비율 |
| --------- | ----------- | ----- | ---- | --------- | --------- |
| 1초       | 512MB       | 42014 | 8290 | 5382      | 22.148%   |

**문제**

서기 2012년! 드디어 2년간 수많은 국민들을 기다리게 한 게임 ACM Craft (Association of Construction Manager Craft)가 발매되었다.

이 게임은 지금까지 나온 게임들과는 다르게 ACM크래프트는 다이나믹한 게임 진행을 위해 건물을 짓는 순서가 정해져 있지 않다. 즉, 첫 번째 게임과 두 번째 게임이 건물을 짓는 순서가 다를 수도 있다. 매 게임시작 시 건물을 짓는 순서가 주어진다. 또한 모든 건물은 각각 건설을 시작하여 완성이 될 때까지 Delay가 존재한다.

 ![img](https://blog.kakaocdn.net/dn/YZxb5/btq0kEKIyXH/fhhg1LMwHE67HapFEipVIk/img.jpg)

위의 예시를 보자.

이번 게임에서는 다음과 같이 건설 순서 규칙이 주어졌다. 1번 건물의 건설이 완료된다면 2번과 3번의 건설을 시작할수 있다. (동시에 진행이 가능하다) 그리고 4번 건물을 짓기 위해서는 2번과 3번 건물이 모두 건설 완료되어야지만 4번건물의 건설을 시작할수 있다.

따라서 4번건물의 건설을 완료하기 위해서는 우선 처음 1번 건물을 건설하는데 10초가 소요된다. 그리고 2번 건물과 3번 건물을 동시에 건설하기 시작하면 2번은 1초뒤에 건설이 완료되지만 아직 3번 건물이 완료되지 않았으므로 4번 건물을 건설할 수 없다. 3번 건물이 완성되고 나면 그때 4번 건물을 지을수 있으므로 4번 건물이 완성되기까지는 총 120초가 소요된다.

프로게이머 최백준은 애인과의 데이트 비용을 마련하기 위해 서강대학교배 ACM크래프트 대회에 참가했다! 최백준은 화려한 컨트롤 실력을 가지고 있기 때문에 모든 경기에서 특정 건물만 짓는다면 무조건 게임에서 이길 수 있다. 그러나 매 게임마다 특정건물을 짓기 위한 순서가 달라지므로 최백준은 좌절하고 있었다. 백준이를 위해 특정건물을 가장 빨리 지을 때까지 걸리는 최소시간을 알아내는 프로그램을 작성해주자.

**입력**

첫째 줄에는 테스트케이스의 개수 T가 주어진다. 각 테스트 케이스는 다음과 같이 주어진다. 첫째 줄에 건물의 개수 N 과 건물간의 건설순서규칙의 총 개수 K이 주어진다. (건물의 번호는 1번부터 N번까지 존재한다) 

둘째 줄에는 각 건물당 건설에 걸리는 시간 D가 공백을 사이로 주어진다. 셋째 줄부터 K+2줄까지 건설순서 X Y가 주어진다. (이는 건물 X를 지은 다음에 건물 Y를 짓는 것이 가능하다는 의미이다) 

마지막 줄에는 백준이가 승리하기 위해 건설해야 할 건물의 번호 W가 주어진다.

**출력**

건물 W를 건설완료 하는데 드는 최소 시간을 출력한다. 편의상 건물을 짓는 명령을 내리는 데는 시간이 소요되지 않는다고 가정한다.

건설순서는 모든 건물이 건설 가능하도록 주어진다.

**제한**

- 2 ≤ N ≤ 1000
- 1 ≤ K ≤ 100,000
- 1 ≤ X, Y, W ≤ N
- 0 ≤ D ≤ 100,000, D는 정수



#### 풀이

문제가 복잡해서 잘 이해해야 한다. 문제가 애매모호 한 것 같아서 내가 가정한 부분은 다음과 같다.

-> 제약이 없는 (제약을 만족한) 모든 건물은 동시에 지을 수 있다.



Input에 대해 특이사항으로는 건물의 제약을  2차원 배열 curriculum에 [ before ] [ after ] 순으로 저장하였다.



우선 단순하게 건물을 짓고 싶을 때 해야하는 과정을 살펴 보자

1. 제약 조건 찾기
2. 제약 조건에 따른 건물이 지어져 있는지 확인
3. 모두가 지어져 있다면 건물을 짓기
4. 제약 조건이 만족하지 않는다면, 먼저 지어야 하는 건물을 먼저 짓기 



그렇다면 이제 건설 시간을 고려해보자.

만약 4번 건물을 지어야 하는데, 제약조건이 1,2,3번 건물인 경우

4번까지 짓는데 걸리는 시간의 최소는 =  1,2,3번 중에서의 최대(병렬 실행이 가능하므로) + 4번을 짓는데 걸리는 시간으로 볼 수 있다.



이때 만약 1,2,3번 건물이 1 -> 2,3	/ 2 -> 3 과 같이 서로 엮여있는 상태의 경우, 중복적인 연산이 있을 수 있으므로 DP를 활용하여 각각의 시간을 저장해 두면 된다.





#### 코드

````java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
	
	static int round;
	static int N;
	static int K;
	static int[] costs;
	static int[][] curriculum;
	static int win;
	
	static int[] dp;
	static int[] answer;
	
   public static void main(String[] args) throws IOException{
	   
	  StringBuilder sb = new StringBuilder();
	  BufferedReader br = new java.io.BufferedReader(new InputStreamReader(System.in));
	  
	  round = Integer.parseInt(br.readLine());
	  answer = new int[round];
	  
	  for(int r=0; r<round; r++) {
		  String[] spl = br.readLine().split(" ");
		  //건물 수
		  N = Integer.parseInt(spl[0]);
		  //건설 순서규칙 개수
		  K = Integer.parseInt(spl[1]);
		  
		  costs = new int[N];
		  spl = br.readLine().split(" "); 
		  //시간
		  for(int i=0; i<N; i++) {
			  costs[i] = Integer.parseInt(spl[i]);
		  }
		  
		  curriculum = new int[N][N];
		  int before;
		  int after;
		  //순서 넣기
		  for(int i=0; i<K; i++) {
			  spl = br.readLine().split(" ");
			  before = Integer.parseInt(spl[0]);
			  after = Integer.parseInt(spl[1]);
			  curriculum[before-1][after-1] = 1;
		  }
		  
		  //필수 건물
		  win = Integer.parseInt(br.readLine());
		  
		  
		  dp = new int[N];
          for(int i=0; i<N; i++) {
              dp[i] = -1;
          }
		  answer[r] = dp(win-1);
	  }
	  
	  //정답 출력
	  for(int r : answer) {
		  System.out.println(r);
	  }
   }
   
   public static int dp(int target) {
	   
	   if(dp[target] != -1) {
		   return dp[target];
	   }
	   int currentCost = 0;
	   //바로 시작 가능하면 바로 짓고, 안되면 제약 건물 짓는데 걸리는 시간 탐색
	   for(int i=0; i<N; i++) {
		   if(curriculum[i][target] == 1) {
			   currentCost = Math.max(currentCost, dp(i));
		   }
	   }
	   dp[target] = currentCost + costs[target];
	   return dp[target];
   }
}
````



#### 결과

![img](https://blog.kakaocdn.net/dn/HwIxw/btq0oaouKE1/53b3Vvvjt811QlJKZMMb00/img.png)