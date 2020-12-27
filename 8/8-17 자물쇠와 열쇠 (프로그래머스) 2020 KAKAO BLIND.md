## 8-16 괄호 변환 (프로그래머스) [JAVA] 2020 KAKAO BLIND RECRUITMENT

#### 문제

##### 문제 설명

고고학자인 **튜브**는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 **자물쇠**로 잠겨 있었고 문 앞에는 특이한 형태의 **열쇠**와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가 **`1 x 1`**인 **`N x N`** 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 **`M x M`** 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
- M은 항상 N 이하입니다.
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
  - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.

##### 입출력 예

| key                               | lock                              | result |
| --------------------------------- | --------------------------------- | ------ |
| [[0, 0, 0], [1, 0, 0], [0, 1, 1]] | [[1, 1, 1], [1, 1, 0], [1, 0, 1]] | true   |



#### 풀이

가장 처음 생각한 방법으로는, 미리 4가지 경우 (키를 돌리는 경우)의 키를 만든 뒤

자물쇠의 범위를 벗어나게 시도할 수 있는데, 영역을 벗어나면 영향을 주지 않는 것을 고려하여

자물쇠의 범위만큼의 배열을 만들고 모든 경우를 체크하는 방법으로 생각했다.

체크는 덧셈으로 해서 1이 아니면 (1+1 = 둘다 막혀있는 경우, 0+0 = 자물쇠로 열지 못한 경우) 실패로 하였다.



완전탐색의 시간복잡도를 고려해볼때,

총 한개씩 걸치는 경우까지 다 고려해서 (N+M-1) * (N+M-1)번의 비교마다 N*N개의 비교 (자물쇠 크기)를 4번 (회전)한다.

$O(N^4)$의 시간복잡도이지만, N과 M이 20이하이므로 충분할 것이라고 생각하고 진행했다. 





#### 코드

````java
public class Main {
	public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    
    public static void main(String[] args)throws Exception {
    	Solution s = new Solution();
    	int[][] key  = {{0,0,0},{1,0,0},{0,1,1}};
    	int[][] lock = {{1,1,1},{1,1,0},{1,0,1}};
    	s.solution(key, lock);
    	
    }
}
class Solution {
	int M, N;
	int[][][] keyset;
	
    public boolean solution(int[][] key, int[][] lock) {
        boolean answer = true;
        M = key.length;
        N = lock.length;
        
        keyset= new int[4][M][M];
        keysetting(key);
        int[][] padlock = padding(lock);
        answer = Able(lock, padlock);
        System.out.println(answer);
        return answer;
    }
    
    public boolean Able(int[][] lock, int[][] padlock) {
    	
    	for(int i=0; i<M+N-1; i++) {
    		for(int j=0; j<M+N-1; j++) {
                //모든 경우에서 키4개 비교
    			if(compare(lock, padlock, keyset[0], i, j) == true) {
    				return true;
    			}
    			if(compare(lock, padlock, keyset[1], i, j) == true) {
    				return true;
    			}
    			if(compare(lock, padlock, keyset[2], i, j) == true) {
    				return true;
    			}
    			if(compare(lock, padlock, keyset[3], i, j) == true) {
    				return true;
    			}
    		}
    	}
    	
    	return false;
    }
    
    public boolean compare(int[][] lock, int[][] padlock, int[][] key, int spy, int spx) {
        //덧셈 연산 (1+1 -> x 0+1 -> o)
    	for(int i=0; i<key.length; i++) {
    		for(int j=0; j<key.length; j++) {
    			padlock[spy+i][spx+j] = padlock[spy+i][spx+j]+key[i][j];		
    		}
    	} 	
    	for(int y=0; y<N; y++) {
			for(int x=0; x<N; x++) {
                //만약 틀렸으면 초기화하고 리턴
				if(padlock[y+M-1][x+M-1] != 1) {
					initialize(lock, padlock);
					return false;
				}
			}
		}
    	return true;
    }
    
    //미리 0 90 180 270 돌린 키 만들어놓기
    public void keysetting(int[][] key) {
    	for(int i=0; i<M; i++) {
    		for(int j=0; j<M; j++) {
    			keyset[0][i][j] = key[i][j];
    			keyset[1][i][j] = key[M-1-j][i];
    			keyset[2][i][j] = key[M-1-i][M-1-j];
    			keyset[3][i][j] = key[j][M-1-i];
    		}
    	}
    }
    
    //자물쇠 초기화
    public void initialize(int[][] lock, int[][] padlock) {	
    	for(int i=0; i<N; i++) {
    		for(int j=0; j<N; j++) {
    			padlock[i+M-1][j+M-1] = lock[i][j];
    		}
    	}
    }
    
    //패딩
    public int[][] padding(int[][] lock){	
    	int len = 2*M+N-2;
    	int[][] padlock =new int[len][len];
    	//1개 걸치는 경우까지 모두 고려
    	for(int i=0; i<len; i++) {
    		for(int j=0; j<len; j++) {
    			padlock[i][j] = 0;
    		}
    	}    	
    	initialize(lock, padlock);    	
    	return padlock;
    }
````

#### 결과

````
테스트 1 〉	통과 (0.42ms, 52.3MB)
테스트 2 〉	통과 (0.31ms, 53.2MB)
테스트 3 〉	통과 (3.65ms, 52.4MB)
테스트 4 〉	통과 (0.28ms, 52.3MB)
테스트 5 〉	통과 (1.41ms, 53.1MB)
테스트 6 〉	통과 (1.08ms, 52.7MB)
테스트 7 〉	통과 (16.43ms, 52.5MB)
테스트 8 〉	통과 (18.83ms, 52.6MB)
테스트 9 〉	통과 (4.81ms, 53.1MB)
테스트 10 〉	통과 (12.75ms, 53.2MB)
테스트 11 〉	통과 (20.38ms, 54.5MB)
테스트 12 〉	통과 (0.27ms, 52.5MB)
테스트 13 〉	통과 (2.70ms, 53MB)
테스트 14 〉	통과 (1.17ms, 52.3MB)
테스트 15 〉	통과 (4.22ms, 53.3MB)
테스트 16 〉	통과 (4.52ms, 52.9MB)
테스트 17 〉	통과 (2.44ms, 53.2MB)
테스트 18 〉	통과 (19.24ms, 53.1MB)
테스트 19 〉	통과 (0.46ms, 53.3MB)
테스트 20 〉	통과 (16.70ms, 53MB)
테스트 21 〉	통과 (8.11ms, 53.3MB)
테스트 22 〉	통과 (3.08ms, 52.9MB)
테스트 23 〉	통과 (1.65ms, 52.8MB)
테스트 24 〉	통과 (1.13ms, 53.6MB)
테스트 25 〉	통과 (23.46ms, 52.5MB)
테스트 26 〉	통과 (25.00ms, 53.1MB)
테스트 27 〉	통과 (25.84ms, 52.6MB)
테스트 28 〉	통과 (2.92ms, 52.3MB)
테스트 29 〉	통과 (2.00ms, 52.6MB)
테스트 30 〉	통과 (5.31ms, 52.7MB)
테스트 31 〉	통과 (12.24ms, 53.2MB)
테스트 32 〉	통과 (19.97ms, 52.7MB)
테스트 33 〉	통과 (6.07ms, 53MB)
테스트 34 〉	통과 (0.59ms, 52.4MB)
테스트 35 〉	통과 (1.07ms, 51.7MB)
테스트 36 〉	통과 (1.17ms, 52.8MB)
테스트 37 〉	통과 (1.34ms, 53.1MB)
테스트 38 〉	통과 (0.36ms, 52.1MB)
````