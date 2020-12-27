## 6-4 : 시계 맞추기 (문제ID : CLOCKSYNC, 난이도 중)



4X4 의 격자 형태로 배치된 16개의 시계가 있습니다. 이 시계들은 모두 12시 3시,6시,9시 를 가리키고 있는데, 이 시계들이 모두 12시를 가리키도록 바꾸고 싶습니다. 10개의 스위치를 조작해 이 시계를 3시간 앞으로 당길 수 있는데, 이 스위치들은 아래 표와 같이 연결되어있습니다.

<table align = "center">
    <tr>
        <th>스위치 번호 </th>
        <th>연결된 시계들 </th>
        <th>스위치 번호 </th>
        <th>연결된 시계들 </th>
    </tr>
    <tr>
        <td>0</td>
        <td>0,1,2</td>
        <td>5</td>
        <td>0,2,14,15</td>
    </tr>
    <tr>
        <td>1</td>
        <td>3,7,9,11</td>
        <td>6</td>
        <td>3,14,15</td>
    </tr>
    <tr>
        <td>2</td>
        <td>4,10,14,15</td>
        <td>7</td>
        <td>4,5,7,14,15</td>
    </tr>
    <tr>
        <td>3</td>
        <td>0,4,5,6,7</td>
        <td>8</td>
        <td>1,2,3,4,5</td>
    </tr>
    <tr>
        <td>4</td>
        <td>6,7,8,10,12</td>
        <td>9</td>
        <td>3,4,5,9,13</td>
    </tr>
</table>

* 입력

  * 테스트 케이스 C (C<=30)
  * 16개의 정수 (12,3,6,9) = 0~15번까지 시계가 지금 가리키고 있는 시간

* 출력

  * 각 테스트 케이스당 스위치를 누를 최소 횟수. (실패하면 -1)

    
  

#### 풀이

1. 10개의 스위치에 따라 16개의 시계가 작동됨을 나타낸 switchList[10] [16] 작성.

2. c가 입력되면, c만큼 함수 solve() 진행

3. solve()에서는 16개의 시계의 시간을  (clock[])에 넣고 최소 횟수를 찾는 switchClock() 실행.

4. switchClock()은 배열 clock[]과 몇번째 스위치를 누를 것인지 정하는 switchnum을 인자로 받음

   > 탈출 조건으로 switchnum ==  switchList.length( = 10) 일때 (스위치 10번째 누르는 것까지 모든 경우의 수를 다했을때) 모든 시계가 12시이면 (isclock12) 0을, 아니면 9999를 return.

5. count = 9999 (성공 못할때) 로 설정해놓고, for문 실행

   > 스위치를 누르는 순서와는 관계없이, 몇번 누르냐에 따라서 문제 해결 가능
   >
   > 이때 스위치를 4번 누르면 한바퀴가 돌아 다시 원점으로 돌아옴. 따라서 10가지 스위치를 0~3번씩 모두 눌러보는 경우만 생각하면 됨.
   >
   > 1. count에 기존의 count와 이번에 버튼 누른 횟수 + switchnum+1으로 다시 돌린 switchClock에서 최솟값을 비교
   > 2. 버튼 누르기 (0~3) // 이때  결국 버튼은 0 1 2 3 4번 눌렸으니 원래의 값으로 되돌아감.(재귀 호출 후 초기화)

6. 버튼 누르는 함수는 switchList[] [] 에서 switchnum 행에서 1인 애 찾아서 그 값에 해당하는 clock[]에 3 더하기.

7. 만약 최종값이 9999 이상이면 (완전탐색 이후에도 해결되지 않았다면) 문제의 조건에 따라 -1로 바꿔주고 출력

   #### 코드(JAVA)

   ```java
   import java.util.ArrayList;
   
   public class CLOCKSYNC {
   	public static StringBuilder sb = new StringBuilder();
   	public static java.util.Scanner sc = new java.util.Scanner(System.in);
   	public static int C;
   	public static int[] clock = new int[16];
   	public static int ans;
   	public static int[][] switchlist = {{1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0},
   										{0,0,0,1,0,0,0,1,0,1,0,1,0,0,0,0},
   										{0,0,0,0,1,0,0,0,0,0,1,0,0,0,1,1},
   										{1,0,0,0,1,1,1,1,0,0,0,0,0,0,0,0},
   										{0,0,0,0,0,0,1,1,1,0,1,0,1,0,0,0},
   										{1,0,1,0,0,0,0,0,0,0,0,0,0,0,1,1},
   										{0,0,0,1,0,0,0,0,0,0,0,0,0,0,1,1},
   										{0,0,0,0,1,1,0,1,0,0,0,0,0,0,1,1},
   										{0,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0},
   										{0,0,0,1,1,1,0,0,0,1,0,0,0,1,0,0}
   										};	
   	public static void main(String[] args)throws Exception {
   	        C = sc.nextInt();
   	        sc.nextLine();
   	        for(int i=0;i<C;i++) {
   	            solve();
   	        }
   	        System.out.println(sb.toString());
   	    }	
   	static void solve() throws Exception {
   		//시계 상태 넣기.
   		for(int i = 0; i < clock.length; i++) {
   			 clock[i] = sc.nextInt();
   		}		
   		ans = switchClock(clock,0);		
   		if(ans >= 9999){
   			ans = -1;
   		}
   		sb.append(ans).append("\n");
   		ans = 0;
   	}
   	//12시인지 체크
   	static boolean isclock12(int[] clock) {
   		
   		for(int i=0; i<clock.length; i++) {
   			if(clock[i] != 12)
   				return false;
   		}
   		return true;
   	}
   	//버튼 누르는 애
   	static void click(int switchnum, int[] clock) {
   		for(int j = 0; j < switchlist[switchnum].length; j++) {
   			if(switchlist[switchnum][j] == 1) {
   				clock[j] = clock[j] + 3;
   			}
   			if(clock[j] == 15) clock[j] = 3;
   		}
   	}
   	static int switchClock(int clock[], int switchnum) throws Exception {	
   		//다 눌렀을 때 
   		if(switchnum == switchlist.length) {
   			return isclock12(clock) ? 0 : 9999; 
   		}
   		
   		int count = 9999;
   		
   		//3일때 한번 더 눌러서 원상태 만들고 다음 재귀함수 돌릴 수있음.
   		for(int i =0; i<4; i++) {
   			count = Math.min(count,i+switchClock(clock,switchnum+1));
   			click(switchnum,clock);
   		}
   		return count;
   	}	
   }
   ```


#### 복기

처음에 시계 기준으로 생각함. 

눌러야 하는 시계 모두 찾기 -> 그 시계들과 최대한 많이 겹치는 버튼 누르기 -> 이걸 계속 진행

이렇게 생각했으나, 너무 어려워서 실패.

두번째는 시계 누르는 순서는 상관 없음을 생각하고

눌러야 하는 시계들을 계속해서 탐색해 0번부터 눌러보려고생각 -> stackoverflow 에러로 실패.

책을 보고( 0 1 2 3 ) 4가지 경우의 수 만 가능하고, for문을 0~3까지 돌리면 초기화 됨을 알아내고 함.

재귀함수를 돌릴때 반환형을 int로 할지 ,void로 할지 여전히 헷갈림

주로 가능한 경우의 수는 void, 최적의 값이나 경로는 int로 하는 것 같음.