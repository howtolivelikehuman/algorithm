## 6-1 : 보글게임(ID : BOOGLE, 난이도 하)

2020.02.11

5X5 크기의 알파벳 격자를 가지고 하는 게임. 상하좌우.다각선으로 인접한 칸들의 글자들을 이어서 단어를 찾아내야함. 각 글자들은 대각선으로 이어질 수 있으며 한 글자가 두번이상 사용 가능. 주어진 칸에서 시작해 특정 단어를 찾을 수 있는지 확인하는 문제.

* hasWord(y,x,word) = 보글 게임판에 (y,x) 에서 시작하는 단어 word의 존재 여부를 반환.

#### 기저사례

1. 위치 (y,x)에 있는 글자가 단어의 첫글자가 아닌 경우 항상 실패.
2. 원하는 단어가 한 글자인 경우 항상 성공.

#### 그 외 조건

​	y>5, x>5여도 안됨. (5x5 경기판이기 때문)

​	**조건을 만족하면 탈출해야함!!**

#### 논리 전개

1. 기저 조건 탐색
2. 글자 맨앞에 한글자 자르고 
3. 반복문으로 8가지 방향(YX의 전후좌우대각선) 돌리기
4. 이때 나머지 조건 만족시키면서 앞글자 날린 word로 hasWord 재귀로 돌리기!

#### 코드(JAVA)

> ```java
> import java.util.Scanner;
> 
> public class BOOGLE {
> 	static char index[][] = {{'U','R','L','P','M'}, {'X','P','R','E','T'},
> 		{'G','I','A','E','T'}, {'X','T','N','Z','Y'}, {'X','O','Q','R','S'}};
> 	//8번 y랑 x가 움직이는거.
> 	static int yMove[] = {1,1,1,0,0,-1,-1,-1};
> 	static int xMove[] = {-1,0,1,-1,1,-1,0,1};
> 	
> 	public static void main(String[] args) {
> 		Scanner sc = new Scanner(System.in);
> 		int posy = sc.nextInt();
> 		int posx = sc.nextInt();
> 		sc.nextLine();
> 		String word = sc.nextLine();
> 		
> 		if((posx-1 < 0)||(posx-1 > 5)||( posy-1 < 0 )||(posy-1 > 5)){
> 			System.out.println("범위가 틀렸습니다!");
> 		}
> 		else{
> 			if (hasWord(posy-1,posx-1,word)){
> 				System.out.println(word + ": 만들기 성공");
> 			}
> 			else {
> 				System.out.println(word+ ": 만들기 실패");
> 			}
> 		}
> 		
> 		
> 		
> 		
> 		
> 	}
> 	
> 	public static boolean hasWord(int y, int x, String word){
> 		//찾아야 하는 문자열을 사이즈 알아내고 한글자씩 나누기.
> 		
> 		//기저 1번 첫글자가 다른 경우 실패
> 		if(index[y][x] != word.charAt(0)) {return false;}
> 	
> 		//기저 2번 한글자 문자일 경우 성공
> 		if(word.length() == 1) {return true;}
> 		
> 		//첫글자가 같고, 단어가 한글자가 아니니깐 단어 맨앞 한글자 지우고 재귀함수 호출.
> 		word = word.substring(1,word.length());
> 		
> 		for(int i = 0; i<8; i++){
> 			// 0<=y<=4 , 0<=x<=4 일때만 진행.
> 			if((y+yMove[i] >= 0)&&(y+yMove[i] <= 4)&&(x+xMove[i] >= 0)&&(x+xMove[i] <= 4)){
> 				if (hasWord(y+yMove[i], x+xMove[i],word)) return true;
> 			}		
> 		}
> 		
> 		return false;
> 	}
> }
> 
> ```
>
> 

근데 이거는 숙제가 아니라구 한다..... 억울해

