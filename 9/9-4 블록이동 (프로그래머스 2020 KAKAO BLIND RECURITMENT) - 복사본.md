## 9-4 블록이동 (프로그래머스 2020 KAKAO BLIND RECURITMENT)

#### 문제

**문제 링크** : https://programmers.co.kr/learn/courses/30/lessons/60063

**문제 설명**

로봇개발자 **무지**는 한 달 앞으로 다가온 카카오배 로봇경진대회에 출품할 **로봇**을 준비하고 있습니다. 준비 중인 로봇은 **`2 x 1`** 크기의 로봇으로 무지는 **0**과 **1**로 이루어진 **`N x N`** 크기의 지도에서 **`2 x 1`** 크기인 로봇을 움직여 **(N, N)** 위치까지 이동 할 수 있도록 프로그래밍을 하려고 합니다. 로봇이 이동하는 지도는 가장 왼쪽, 상단의 좌표를 **(1, 1)**로 하며 지도 내에 표시된 숫자 **0**은 빈칸을 **1**은 벽을 나타냅니다. 로봇은 벽이 있는 칸 또는 지도 밖으로는 이동할 수 없습니다. 로봇은 처음에 아래 그림과 같이 좌표 **(1, 1)** 위치에서 가로방향으로 놓여있는 상태로 시작하며, 앞뒤 구분없이 움직일 수 있습니다.



로봇이 움직일 때는 현재 놓여있는 상태를 유지하면서 이동합니다. 예를 들어, 위 그림에서 오른쪽으로 한 칸 이동한다면 **(1, 2), (1, 3)** 두 칸을 차지하게 되며, 아래로 이동한다면 **(2, 1), (2, 2)** 두 칸을 차지하게 됩니다. 로봇이 차지하는 두 칸 중 어느 한 칸이라도 **(N, N)** 위치에 도착하면 됩니다.

로봇은 다음과 같이 조건에 따라 회전이 가능합니다.



위 그림과 같이 로봇은 90도씩 회전할 수 있습니다. 단, 로봇이 차지하는 두 칸 중, 어느 칸이든 축이 될 수 있지만, 회전하는 방향(축이 되는 칸으로부터 대각선 방향에 있는 칸)에는 벽이 없어야 합니다. 로봇이 한 칸 이동하거나 90도 회전하는 데는 걸리는 시간은 정확히 1초 입니다.

**0**과 **1**로 이루어진 지도인 board가 주어질 때, 로봇이 **(N, N)** 위치까지 이동하는데 필요한 최소 시간을 return 하도록 solution 함수를 완성해주세요.





**제한사항**

- board의 한 변의 길이는 5 이상 100 이하입니다.
- board의 원소는 0 또는 1입니다.
- 로봇이 처음에 놓여 있는 칸 (1, 1), (1, 2)는 항상 0으로 주어집니다.
- 로봇이 항상 목적지에 도착할 수 있는 경우만 입력으로 주어집니다.

------

**입출력 예**

| board                                                        | result |
| ------------------------------------------------------------ | ------ |
| [[0, 0, 0, 1, 1],[0, 0, 0, 1, 0],[0, 1, 0, 1, 1],[1, 1, 0, 0, 1],[0, 0, 0, 0, 0]] | 7      |



#### 풀이

최단경로를 찾아야 하기 때문에 BFS 방식을 우선 사용하기로 했다.



로봇이 두 칸을 차지하고, 가능한 가짓수가

상-하-좌-우 이동 (4가지)와 회전 (축A, 시계) (축B, 시계), (축A, 반시계) (축B, 반시계)로 4가지로 

각각의 상황에 맞게 최적의 동작을 하는 것보다, BFS를 통해 모든 가능동작을 고려해야 한다고 생각했다.



이때 로봇이 가로라고 가정했을때

반시계 방향에서의 회전을 축을 a,b라 할때 축이 아닌 좌표의 이동위치를 생각해 보면 다음과 같다.

시계 방향에서는 또 반대로이고, 로봇이 세로일때는 또 시계방향, 반시계방향에서 좌표가 반대이다.

하지만 어차피 2개의 축에대해 2가지 방향 4번을 무조건 다 실행해 볼 것이기 때문에, 추가로 고려하지 않았다.



기존의 BFS라면 지나온 경로를 체크함으로써 무한루프를 방지하는데, 이번 문제같은 경우 로봇이 2칸이고, 회전의 경우가 있기 때문에 배열의 개별 좌표를 체크하는 형식으로는 힘들다.

따라서 해시맵을 사용하여 좌표를 문자열로 변환시킨 키값으로 사용하여 겹치지 않도록 했다.



또한 좌표의 순서가 (1,1 - 2,1), (2,1 - 1,1)의 경우 중복이 될 수 있기 때문에 항상 뒤의 좌표가 y, x순으로 더 크게 유지하도록 했다.





#### 코드

````java
import java.util.HashMap;
import java.util.LinkedList;
import java.util.Queue;

class Solution {
	int[][] point = {{0,0},{0,1}};
	int[][] board;
	int N = 0;
	HashMap<String,Integer> paths = new HashMap<String,Integer>();
	
    public int solution(int[][] board) {
    	N = board.length;
    	this.board = board;
    	path = new int[N][N];
    	
    	
    	Queue<int[][]> queue = new LinkedList<int[][]>();
    	paths.put(""+point[0][0] +"-"+ point[0][1] +"-"+ point[1][0] +"-"+ point[1][1], 1);
    	queue.add(point);
    	
    	int answer = 0;
    	int size =1;
    	boolean breakcode = false;
    	
    	while(!queue.isEmpty()) {
    		
    		size = queue.size();
            
    		for(int a = 0; a<size; a++) {
    			point = queue.poll();
        		
        		if((point[1][0] == N-1 && point [1][1] == N-1)) {
        			breakcode = true;
        			break;
        		}
        		    		
        		//movement
        		for(int i=0; i<4; i++) {
        			int[][] newpoint = movement(i);
        			if(newpoint != null) { 
        				String key = ""+newpoint[0][0] +"-"+ newpoint[0][1] +"-"+ newpoint[1][0] +"-"+ newpoint[1][1];
                        if(!paths.containsKey(key)) {
                            queue.add(newpoint);
                            paths.put(key, answer);
                        }
        			}
        		}
        		
        		//rotate
        		for(int i=0; i<2; i++) {
        			for(int j=0; j<2; j++) {
        				int[][] newpoint = Checkrotate(i,j);
        				if(newpoint != null) {
    						String key = ""+newpoint[0][0] +"-"+ newpoint[0][1] +"-"+ newpoint[1][0] +"-"+ newpoint[1][1];
    						if(!paths.containsKey(key)) {
        						queue.add(newpoint);
        						paths.put(key, answer);
        					}
        				}
        			}
        		}
    		}
    		
    		if(breakcode) return answer;
    		answer++;
    	}
        return answer;
    }
    
    public void print(int[][] point) {
    	for(int i=0; i<2; i++) {
    		for(int j=0; j<2; j++) {
    			System.out.print(point[i][j] + " ");
    		}
    	}
    	System.out.println();
    }
    
    public int[][] movement(int direction){
    	int y1=0;
    	int y2=0;
    	int x1=0;
    	int x2=0;
    	
    	switch(direction) {
    	//up
    	case 0:
    		y1 = point[0][0] -1;
			y2 = point[1][0] -1;
			x1 = point[0][1];
			x2 = point[1][1];
    		break;
    	//down
    	case 1:
    		y1 = point[0][0] +1;
			y2 = point[1][0] +1;
			x1 = point[0][1];
			x2 = point[1][1];
    		break;
    	//left
    	case 2:
    		y1 = point[0][0];
			y2 = point[1][0];
			x1 = point[0][1]-1;
			x2 = point[1][1]-1;
    		break;
    	//right
    	case 3:
    		y1 = point[0][0];
			y2 = point[1][0];
			x1 = point[0][1]+1;
			x2 = point[1][1]+1;
    		break;
    	}
    	
    	if(y1 >=0 && y2>=0 && x1>=0 && x2>=0 && y1 < N && y2 < N && x1 < N && x2 < N ) {
    		if(board[y1][x1] == 0 && board[y2][x2] == 0) {
    			int newpoint[][] = new int[2][2];
    			
    			if((y1 == y2)) {
    				if(x1 > x2) {
    					newpoint[0][0] = y2; newpoint[0][1] = x2;
    	    			newpoint[1][0] = y1; newpoint[1][1] = x1;
    				}
    				else {
    					newpoint[0][0] = y1; newpoint[0][1] = x1;
            			newpoint[1][0] = y2; newpoint[1][1] = x2;
    				}
    			}
    			else if(y1 > y2) {
    				newpoint[0][0] = y2; newpoint[0][1] = x2;
	    			newpoint[1][0] = y1; newpoint[1][1] = x1;
    			}
    			else {
    				newpoint[0][0] = y1; newpoint[0][1] = x1;
        			newpoint[1][0] = y2; newpoint[1][1] = x2;
    			}
    			return newpoint;
    		}
    	}
    	return null;
    }
      
    public int[][] Checkrotate(int direction, int p) {
    	int y=0;
    	int x=0;
    	int ry =0;
    	int rx =0;
    	
    	switch(direction) {
    	//반시계
    	case 0:
    			y = point[1-p][0] + point[p][1] - point[1-p][1];
        		x = point[1-p][1] + point[p][0] - point[1-p][0];
        		
        		ry = point[p][0] - point[1-p][1] + point[p][1];
        		rx = point[p][1] - point[1-p][0] + point[p][0];
    		
    		break;
    		
    	//시계
    	case 1:
    			y = point[1-p][0] - point[p][1] + point[1-p][1];
        		x = point[1-p][1] - point[p][0] + point[1-p][0];
        		
        		ry = point[p][0] + point[1-p][1] - point[p][1];
        		rx = point[p][1] + point[1-p][0] - point[p][0];
    		break;
    	}
    	
    	//System.out.println(y +","+x + " --" + point[p][0] +","+point[p][1]+" " +ry+","+rx);
    	
    	if(y >=0 && y< N && x >=0 && x < N && ry >= 0 && rx >= 0 && rx < N && ry < N) {
    		if((board[y][x] == 0) && (board[ry][rx] == 0)) {
    			int newpoint[][] = new int[2][2];
	
    			if(point[p][0] == ry) {
    				if(point[p][1] > rx) {
    					newpoint[0][0] = ry; newpoint[0][1] = rx;
    	    			newpoint[1][0] = point[p][0]; newpoint[1][1] = point[p][1];
    				}
    				else {
    					newpoint[1][0] = ry; newpoint[1][1] = rx;
    	    			newpoint[0][0] = point[p][0]; newpoint[0][1] = point[p][1];
    				}
    			}
    			else if(point[p][0] > ry) {
    				newpoint[0][0] = ry; newpoint[0][1] = rx;
	    			newpoint[1][0] = point[p][0]; newpoint[1][1] = point[p][1];
    			}
    			else {
    				newpoint[1][0] = ry; newpoint[1][1] = rx;
	    			newpoint[0][0] = point[p][0]; newpoint[0][1] = point[p][1];
    			}
    			return newpoint;
    		}
    	}
    	return null;
    }
}
````



#### 결과