## 7-12 Gaaaaaaaaaarden(백준 18809)

| 시간 제한 | 메모리 제한 | 제출 | 정답 | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :--- | :--- | :-------- | :-------- |
| 2 초      | 512 MB      | 776  | 328  | 214       | 39.051%   |

## 문제

길고 길었던 겨울이 끝나고 BOJ 마을에도 봄이 찾아왔다. BOJ 마을에서는 꽃을 마을 소유의 정원에 피우려고 한다. 정원은 땅과 호수로 이루어져 있고 2차원 격자판 모양이다.

인건비 절감을 위해 BOJ 마을에서는 직접 사람이 씨앗을 심는 대신 초록색 배양액과 빨간색 배양액을 땅에 적절하게 뿌려서 꽃을 피울 것이다. 이 때 배양액을 뿌릴 수 있는 땅은 미리 정해져있다.

배양액은 매 초마다 이전에 배양액이 도달한 적이 없는 인접한 땅으로 퍼져간다.

아래는 초록색 배양액 2개를 뿌렸을 때의 예시이다. 하얀색 칸은 배양액을 뿌릴 수 없는 땅을, 황토색 칸은 배양액을 뿌릴 수 있는 땅을, 하늘색 칸은 호수를 의미한다.

<img src="https://upload.acmicpc.net/6c58580b-a750-4824-a9a0-2dd79eab545b/-/preview/" alt="img" style="zoom:67%;" />

초록색 배양액과 빨간색 배양액이 동일한 시간에 도달한 땅에서는 두 배양액이 합쳐져서 꽃이 피어난다. 꽃이 피어난 땅에서는 배양액이 사라지기 때문에 더 이상 인접한 땅으로 배양액을 퍼트리지 않는다.

아래는 초록색 배양액 2개와 빨간색 배양액 2개를 뿌렸을 때의 예시이다.

<img src="https://upload.acmicpc.net/f396d82b-ce1d-42f6-a43b-49ddff720d64/-/preview/" alt="img" style="zoom:67%;" />

배양액은 봄이 지나면 사용할 수 없게 되므로 주어진 모든 배양액을 남김없이 사용해야 한다. 예를 들어 초록색 배양액 2개와 빨간색 배양액 2개가 주어졌는데 초록색 배양액 1개를 땅에 뿌리지 않고, 초록색 배양액 1개와 빨간색 배양액 2개만을 사용하는 것은 불가능하다.

또한 모든 배양액은 서로 다른 곳에 뿌려져야 한다.

정원과 두 배양액의 개수가 주어져있을 때 피울 수 있는 꽃의 최대 개수를 구해보자.

#### 입력

첫째 줄에 정원의 행의 개수와 열의 개수를 나타내는 N(2 ≤ N ≤ 50)과 M(2 ≤ M ≤ 50), 그리고 초록색 배양액의 개수 G(1 ≤ G ≤ 5)와 빨간색 배양액의 개수 R(1 ≤ R ≤ 5)이 한 칸의 빈칸을 사이에 두고 주어진다.

그 다음 N개의 줄에는 각 줄마다 정원의 각 행을 나타내는 M개의 정수가 한 개의 빈 칸을 사이에 두고 주어진다. 각 칸에 들어가는 값은 0, 1, 2이다. 0은 호수, 1은 배양액을 뿌릴 수 없는 땅, 2는 배양액을 뿌릴 수 있는 땅을 의미한다.

배양액을 뿌릴 수 있는 땅의 수는 R+G개 이상이고 10개 이하이다.

#### 출력

첫째 줄에 피울 수 있는 꽃의 최대 개수를 출력한다.

#### 풀이

문제는 크게 2가지로 나눌 수 있다.

1. 배양액을 뿌릴 수 있는 자리에 R개의 묶음과 G개의 묶음을 뿌리기
2. 배양액을 뿌린 뒤 정원의 모습.

우선 배양액을 뿌릴 수 있는 자리를 K라고 할때, K개의 숫자중에 R개의 묶음과 G개의 묶음을 나누는 방법을 해야한다. 

이걸 신경쓰지 않고 모든 경우의 수를 해볼 심산이면, $K!$개라는 어마어마한 숫자를 해야하지만, 조합을 이용하면 ${K\choose R+G} * {R+G\choose R}$ 로 확 줄일 수 있다.

조합을 구현할때는 , 0~K개까지 숫자들에서 index를 잡고, index를 포함해서 뽑는 방법, index를 제외하고 뽑을 방법 2가지를 재귀적으로 반복하게 했다.

우선 K개  중 R+G개를 뽑는 조합을 하고, 바로 R+G개 중 R개를 뽑은 뒤, 차례대로 큐에 R과 G를 나누어 넣는 조합2까지 실행했다.

이때 큐에 배양액을 뿌릴 위치(R과 G의 위치들)을 넣을때, 정원과 (GARDEN), 시간(VISITED) 역시 바로 체크해주었다.

배양액을 뿌리는 것은 BFS 방식으로 탐색했는데 , 큐의 크기를 확인해서 그만큼 도는것을 한 시간으로 둔 다음 탐색을 시작하는데,

배양액의 위치에서 퍼질 곳(전후좌우)의 시간과 장소를 확인해 퍼지던가, 꽃이 생기던가 하는 방식으로 탐색했다.



시간복잡도

1. (조합) K개 중 R개를 뽑는게 $O(2^R)$, 이 R개중 G개를 뽑는게 $O(2^G)$이므로 셋다 그냥 k라고 통칭하면 $O(2^{2k})$ 

2. (배양액) $2차원 배열을 N*N이라고 할때 DFS 탐색이므로 O(N^2)$ 

   따라서 $O(2^{2k} * N^2)$ 인데,  K가 10 이하에, 실제로 R과 G는 5 이하이므로 빠른 시간에 해결할 수 있었다.

#### 코드

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Stack;

public class Main {
	
	public static class location{
		int x;
    	int y;
		public location(int y , int x) {
    		this.x = x;
    		this.y = y;
    	}
    }
    public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    static int[][] garden;
    static int[][] visited;
    static int dy[] = { 1,-1,0,0 };
    static int dx[] = { 0,0,1,-1 };
    static int ans = 0;
    static int M;
    static int N;
    static int G;
    static int R;
    static Stack<Integer> combinationSet = new Stack<Integer>();
    static ArrayList<location> CanSparge = new ArrayList<location>();
    static Queue<location> WheretoSparge = new LinkedList<location>();
    
    public static void main(String[] args)throws Exception {
    	 String lines = br.readLine();
         String[] spl  = lines.split(" ");
         N=Integer.parseInt(spl[0]);
         M=Integer.parseInt(spl[1]);
         G=Integer.parseInt(spl[2]);	
         R=Integer.parseInt(spl[3]); 
         
         garden = new int[N][M];
         visited = new int[N][M];
         
         for(int i=0; i<N; i++) {
        	 lines = br.readLine().trim();
             spl = lines.split(" ");
             for(int j=0; j<M; j++) {
             	garden[i][j] = Integer.parseInt(spl[j]);
             	//배양액 살포 가능한 땅 저장하기
             	if(garden[i][j] ==2) {
             		CanSparge.add(new location(i,j));
             		garden[i][j] = 1;
             	}
             	visited[i][j] = 9876;
             }
         }
         choose();
         System.out.println(ans);
    }

	public static void spread() {
	    
		location liquid;
		int flowers = 0;
		int time = 1;
		
		while(!WheretoSparge.isEmpty()) {
            //이거 한번 돌때마다 1 time
			int size = WheretoSparge.size();
			for(int i=0; i< size; i++) {
				
				liquid = WheretoSparge.poll();
				int x = liquid.x;
				int y = liquid.y;
                
				//꽃이면 continue
				if(visited[y][x] == -1) continue;
                
				for(int k =0; k< 4; k++) {
					int nextdy = y + dy[k];
					int nextdx = x + dx[k];
					
                    //얘가 퍼질곳의 범위 확인
					if(nextdy > N-1 || nextdy < 0 || nextdx > M-1 || nextdx < 0) continue;
                    //물이면 패스
					if(garden[nextdy][nextdx] == 0)  continue;
					
					if(garden[nextdy][nextdx] == 1) {
						WheretoSparge.add(new location(nextdy, nextdx));
                        //만약에 퍼지면, 퍼졋다고 미리 체크(다른 배양액이 퍼지지 못하게)
						garden[nextdy][nextdx] = garden[y][x];
						visited[nextdy][nextdx] = time;
					}
                    //만약에 똑같은 시간에 퍼졌고, 같은 배양액이 아니면 == 꽃이 생김
					else if(visited[nextdy][nextdx] == time && garden[nextdy][nextdx] != garden[y][x]) {
						garden[nextdy][nextdx] = 10;
						visited[nextdy][nextdx] = -1;
						flowers++;
					}					
				}				
			}
			//시간 증가
			time++;
		}

		//기존의 정원으로 되돌리기
		for(int y = 0; y < N; y++) {
			for(int x = 0 ; x < M; x++) {
				if(garden[y][x] > 1) garden[y][x] = 1;
				visited[y][x] = 9876;
			}
		}
		ans = Math.max(ans, flowers);
	}
	
    public static void choose() {
    	int[] arr = new int[R+G];
    	combination(arr, 0, CanSparge.size(), R+G, 0);
    }
    
	//R+G개 조합
	public static void combination(int[] arr, int index, int n, int r, int target) {
		if (r==0) {
			int[] red = new int[R];
			combination2(red, 0, R+G, R, 0, arr);
		}
		else if (target == n) return;
		else {
			arr[index] = target;
			combination(arr, index+1, n, r-1, target+1);
			combination(arr, index, n, r, target+1);
		}
	}
	
	//R개 조합
	public static void combination2(int [] arr, int index, int n, int r, int target, int[] range) {
		if (r==0) {
			for(int i = 0; i < arr.length; i++) {
				//넣을때 부터 정원, 방문횟수 초기화
				garden[CanSparge.get(range[arr[i]]).y][CanSparge.get(range[arr[i]]).x] = 11;
				visited[CanSparge.get(range[arr[i]]).y][CanSparge.get(range[arr[i]]).x] = 0;
				WheretoSparge.add(CanSparge.get(range[arr[i]]));
			}
			for(int i = 0; i < range.length ; i++) {
				for(int i2 = 0; i2 < arr.length; i2++) {
					if(i == arr[i2]) i++;
				}
				if(i >= range.length) break;
				//넣을때 부터 정원, 방문횟수 초기화
				garden[CanSparge.get(range[i]).y][CanSparge.get(range[i]).x] = 99;
				visited[CanSparge.get(range[i]).y][CanSparge.get(range[i]).x] = 0;
				WheretoSparge.add(CanSparge.get(range[i]));
			}
			spread();
			WheretoSparge.clear();	
			return;
		}
		else if (target == n) return;
		else {
			//range의 주소가 들어감
			arr[index] = target;
			combination2(arr, index+1, n, r-1, target+1, range);
			combination2(arr, index, n, r, target+1, range);
		}
	}	
	public static void print(int[][] board) {
        StringBuilder sb2 = new StringBuilder();
		for(int y = 0; y < N; y++) {
			for(int x = 0 ; x < M; x++) {
				sb2.append(board[y][x]+" ");
			}
			sb2.append("\n");	
		}
		System.out.println(sb2.toString());
	}
}
```

#### 결과

| 채점 번호 | 아이디                                                       | 문제 번호                                      | 결과             | 메모리 | 시간 | 언어                                                         | 코드 길이 | 제출한 시간                                                  |
| :-------- | :----------------------------------------------------------- | :--------------------------------------------- | :--------------- | :----- | :--- | :----------------------------------------------------------- | :-------- | :----------------------------------------------------------- |
| 18734941  | [howtolivelikehuman](https://www.acmicpc.net/user/howtolivelikehuman) | [18809](https://www.acmicpc.net/problem/18809) | **맞았습니다!!** | 281588 | 972  | [Java](https://www.acmicpc.net/source/18734941) / [수정](https://www.acmicpc.net/submit/18809/18734941) | 4571      | [12일 전](https://www.acmicpc.net/status?problem_id=18809&user_id=howtolivelikehuman&language_id=-1&result_id=-1&from_mine=1#) |

사실 아직까지도 이전에 왜 틀렸는지는 모르겠다.