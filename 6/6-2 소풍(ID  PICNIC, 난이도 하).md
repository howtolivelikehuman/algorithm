## 6-2 :소풍(ID : PICNIC, 난이도 하)

2020.02.11

소풍 때 서로 친구인 학생들끼리만 짝을 지어야 합니다. 각 학생들의 쌍에 대해 이들이 서로 친구인지 여부가 주어질 때, 학생들을 짝 지을 수 있는 방법의 수를 계산하는 프로그램을 작성하세요. 짝이 되는 학생들이 일부만 다르더라도 다른 방법이라고 봅니다.

​	(태연 제시카)(써니 티파니)(효연 유리)

​	(태연 제시카)(써니 유리)(호연 티파니)  --> 다른경우

* 입력
  * 테스트 케이스의 수 C (C<=50) 가 주어집니다. 
  * 첫줄에는 학생의 수 n (2<=n<=10) 친구 쌍의 수 m (0<= m <= n(n-1)/2)
  * 그 다음줄에 m개의 정수 쌍으로 서로 친구인 두 학생의 번호 (0부터 n-1사이의 정수)

#### 풀이

1. 친구 쌍을 2차원 배열 friendList[ ] [ ]에 저장 ,  [a] [b] 가 친구면 true를 넣어둠. / [b] [a] 역시 마찬가지

2. 학생의 수(n) 만큼 ArrayList형 friendsCheck에 1저장 (1 = 친구 쌍이 없음 / 99 = 친구 쌍 있음)

3. 재귀 함수 isFriend(index, friendList[] []) 실행

   > 1.  index를 0부터 n까지 탐색 n까지 도달했으면 성공 (친구를 끝까지 다 찾음)
   >
   > 2. 만약에 현재 인덱스에 있는 애 (friendsCheck 에서 index 주소에 해당하는 애) 가 친구 쌍이 있으면
   >
   >    다음 인덱스 애를 탐색
   >
   > 3. 현재 인덱스에 해당하는 애가 친구가 없으면, 그 인덱스랑 친구인 애들을 다 탐색해봄
   >
   > 4. 이때 친구인 애 찾으면 친구 쌍을 만들고, 다음 인덱스로 넘어감.
   >
   >     *이때 다시 돌아와서 다른 경우의 수도 찾아봐야 함으로, 다음 재귀함수 호출 후 직전 꺼 초기화



ex)   0이랑 친구인 애 -> 1이 친구임 (0,1) -> 1은 이미 친구쌍 있으니깐 2랑 친구인 애 -> 3이 친구임(0,1) (2,3)

​		->3은 이미 친구쌍 있으니깐 4랑 친구인 애 -> 5랑 친구임 (0,1)(2,3)(4,5) -> 5까지 왔으니깐 정답

​		다시 돌아가서 2랑 친구인 두번째 애 -> 4가 친구임 (0,1) (2,4) >>..

​    	다시 크게 돌아가서 0이랑 친구인 두번째 애 -> 2가 친구임 (0,2) >>...

#### 코드(JAVA)

```java

import java.util.ArrayList;
import java.util.Scanner;

public class PICNIC {
	static int n;
	static int ans;
	static ArrayList<Integer> friendsCheck = new ArrayList<>();
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int C = sc.nextInt();
		int result[] = new int[C];
		sc.nextLine();
		for(int i = 0; i<C; i++){
			ans = 0;
			n = sc.nextInt();
			//친구쌍이 맺어졌는지를 체크하는 리스트 선언
			for(int j=0; j<n; j++){
				friendsCheck.add(j,1);
			}
			int m = sc.nextInt();
			sc.nextLine();
			
			boolean friendList[][] = new boolean[10][10];
			
			for(int j =0; j<m; j++){
				int a = sc.nextInt();
				int b = sc.nextInt();
				//친구 리스트에 친구라고 표시해두기.
				friendList[b][a] = true;
				friendList[a][b] = true;
			}
			//0부터 재귀함수 실행
			isFriend(0,friendList);
			result[i] = ans;
			
		}
	
		for(int i = 0; i<C; i++){
			System.out.println(result[i]);
		}
	}

		
	//n = 학생 수  m = 친구 쌍, index = list에서의 좌표. 직전에 몇번까지 찾아봤나 넘겨줌.
	public static void isFriend(int index, boolean friendList[][]){
		
		//숫자 끝까지 다 했으면 1번 완료
		if(index == n){
			System.out.println("한번 성공");
			ans = ans +1;
			return;
		}
		//만약 이미 얘가 짝이 있는경우 다음 애 탐색
		if(friendsCheck.get(index) == 99){
			isFriend(index+1,friendList);
			return;
		}
		for(int i = index+1; i<n; i++){
			//무조건 0이랑 친구인애 찾는거부터 시작. -> 시작할때 index가 0이므로
			if((friendsCheck.get(i) == 1) && (friendList[index][i] == true)){
				//만약에 짝 찾으면 해당하는 arraylist 자리에 1대신 99넣기
 				friendsCheck.remove(i);
				friendsCheck.add(i, 99);
				isFriend(index+1,friendList); // 다음 친구 탐색
				//직전에 했던 친구는 다시 돌아와서 탐색해야 함으로 초기화
				friendsCheck.remove(i);
				friendsCheck.add(i,1);
			}
		}
	}
}```
```



#### 복기

처음에 친구 쌍 위주로 조합을 만든 뒤 arraylist에 쌍이 맺어진 인원들을 삭제해가면서 조합을 만들려고 함 

-> 근데 그러면 한가지 조합씩만 만들고 다음으로 넘어가버림.

-> 매 친구 쌍 마다 가능한 조합들로 하려 했으나, 중복되는 경우를 제외시키는걸 구현하기 어려움.

-> 그래서 사람 위주로 생각함.