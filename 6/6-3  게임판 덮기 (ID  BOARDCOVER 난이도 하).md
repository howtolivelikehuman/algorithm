## 6-3 : 게임판 덮기 (ID : BOARDCOVER 난이도 하)

2020.02.13

H x W 크기의 게임판이 있을때, 검은칸(#)과 흰칸(.)으로 구성되어 있음. 이중 모든 흰 칸을 세칸짜리 L자 모양의 블록으로 덮어야 함. 블록을 돌릴 수 있지만, 겹치거나 칸을 나가면 안됨. 이때 덮는 방법의 수를 계산하는 문제.

* 입력  
  * 테스트 케이스 C(c<=30)
  * 각 테스트 케이스의 처줄에는 두개의 정수 H, W (1<=H,W<=20)
  * 그 다음에는 #과 .으로 이루어진 게임판  ( . <=50);



#### 풀이

 1.  c가 입력되면, c만큼 함수 solve 진행

 2. solve 에서는 H와 W를 입력받고 initBoard()로  2차원 배열 board 생성 후 #으로 초기화.

    >  이때 initBoard에서는 실제 H와 W보다 가로세로 1줄이 추가된 크기(H+2,W+2)만큼으로 생성
    >
    > -> 이후의 블럭 탐지할때  outofbound 오류를 막기 위해

 3.  이후 한줄씩 입력받아 board[] []에 게임판 저장

    > 이때 board의 (1,1)에서부터 저장 -> 아까 initBoard()로 게임판의 둘레를 #으로 한줄 더 둘렀기 때문
    >
    > 그리고 blank에 .의 개수를 세어서 저장

 4. blank가 3의 배수이면 ,블럭을 채우는 boardCover() 실행.  

    > L자 블록으로 다 채울려면 빈 칸이 3의 배수 여야 하기 때문

 5. boardCover()에서는 먼저 (1,1)부터 흰 점이 있는지 탐색

 6. 이때 마지막 x, y의 좌표가 ( H+1, W+1 ) 즉 보드판 게임 끝까지 가있는 상태면 모든 판을 다 채웠으므로 성공

 7. 흰 점에서는 4가지 상태로 블럭이 놓일 수 있음

    > @				 @		 @ㅇ		@ㅇ		 -> 비어있는 칸의 가장 위에서 부터였기 때문
    >
    > ㅇㅇ		ㅇㅇ			ㅇ		 ㅇ 			
    >
    > 이 4가지 상태에서의 모든 나머지 두 좌표가 비어있는지 체크. 
    >
    > 이때 만약 나머지 두 좌표가 기존에 주어진 보드게임판보다 바깥에 위치할 수 있었으므로, 한줄씩 더 늘려서 한 것.

 8. 만약 블럭을 놓을 수 있으면 다시 boardCover() 호출 (재귀)

 9. boardCover() 호출 이후에는 다시 직전에 채웠던 칸 비우기 (# -> .) 

 10. 성공시 cover 하나씩 늘려서 저장.

#### 코드(JAVA)

```java

public class BOARDCOVER {
    public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    public static java.util.Scanner sc = new java.util.Scanner(System.in);
    public static int C;
    public static int H;
    public static int W;
    public static int count =0;

    public static String[][] board;
    public static void main(String[] args)throws Exception {
        C=Integer.parseInt(br.readLine().trim());

        for(int i=0;i<C;i++) {
            solve();
        }
        System.out.println(sb.toString());
    }

    public static void solve()throws Exception {

        String lines = br.readLine();
        String[] spl  = lines.split(" ");
        H=Integer.parseInt(spl[0]);
        W=Integer.parseInt(spl[1]);

        initBoard();
        int blank = 0;
        //문제를 1,1부터 넣는 이유 : initBoard에서 지금 넣을 애들 둘레를 한번 #으로 돌렸기 때문에 실제 범위는 둘레 안쪽이 됨.
        for(int i=0;i<H;i++){
            String input = br.readLine();
            int j=1;
            for(char c : input.toCharArray()){
                board[i+1][j] = String.valueOf(c);
                //blank는 블럭을 채울 빈 공간을 미리 세둠
                if(board[i+1][j] =="."){
                	blank++;
                }
                j++;
            }

        }
        //blank가 3의 배수이면 블럭 채우는걸 실행
        if(blank%3 == 0){
            boardCover();
        }
        sb.append(count).append("\n");
    }

    public static void boardCover(){
    	//맨 처음으로 비어있는 점 찾아서 좌표 반환하기
        int x=1;
        int y=1;
        for(x=1;x<H+1;x++){
            for(y = 1; y < W+1; y++) {
            	//맨 처음으로 비어있는 점이면 for문 탈출
                if (board[x][y].equals(".")) {
                    break;
                }
            }
            //2중 for문이니깐 한번더탈출.
            if (y<W+1&&board[x][y].equals(".")) {
                break;
            }
        }
        //만약에 x랑 y가 둘다 끝까지 도달 (#으로 모든 .을 다 채웠다)
        if(x == H+1 && y == W+1){
            count++;
            return ;
        }

        // *
        // @ @
        if(board[x+1][y].equals(".")&&board[x+1][y+1].equals(".")){
            board[x][y] = board[x+1][y] =board[x+1][y+1] = "*";
            boardCover();
            board[x][y] = board[x+1][y] =board[x+1][y+1] = ".";
        }
        // *
        //@@
        if(board[x+1][y-1].equals(".")&&board[x+1][y].equals(".")){
            board[x][y] = board[x+1][y-1] =board[x+1][y] = "*";
            boardCover();
            board[x][y] = board[x+1][y-1] =board[x+1][y] = ".";
        }
        // *@
        //  @
        if(board[x][y+1].equals(".")&&board[x+1][y+1].equals(".")){
            board[x][y] = board[x][y+1] =board[x+1][y+1] = "*";
            boardCover();
            board[x][y] = board[x][y+1] =board[x+1][y+1] = ".";
        }
        // *@
        // @
        if(board[x+1][y].equals(".")&&board[x][y+1].equals(".")){
            board[x][y] = board[x+1][y] =board[x][y+1] = "#";
            boardCover();
            board[x][y] = board[x+1][y] =board[x][y+1] = ".";
        }

    }


    public static void initBoard(){
        
        board = new String[H+2][W+2];
        for(int i=0;i<=H+1;i++){
        	for(int j =0; j<=W+1; j++){
        		board[i][j] = "#";
        	}
        }
        count=0;
    }
```



#### 복기

첫 번째 빈자리 부터 찾아서 4가지 타입의 블럭을 넣을 수 있다고 생각함

블럭을 넣을 때, 일일히 다음, 그다음 칸 역시 빈칸인지를 확인 하는 것을 for문으로 매 칸마다 해버림

->한꺼번에 확인을 안하고 각 칸마다 확인하고 채워버려서, 블럭이 꼬여버리는 상황이 발생함.

지정된 한 칸에서 블럭을 넣을 때, 나머지 2칸이 모두 빈 칸인지를 확인하는데 모든 조건식을 작성하는 것 외에는 방법이 생각나지 않음.

-> 또 그러기에는 배열 범위를 벗어나 버리는 오류도 발생해버림.



결국 인터넷에 쳐봤는데, 숫자를 이용하는 방법은 이해하지 못하고, 결국 노가다 한 방법을 참고함.

배열도 한줄 둘러싸서 만들어버림.



문제 알고리즘과는 관계없이 차용한 코드에서 scanner 대신 bufferreader

정답을 배열 대신 Stringbuilder에 저장하는 방식도 배워야겠다. 