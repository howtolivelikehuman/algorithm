## 9-3 외벽점검 (프로그래머스 2020 KAKAO BLIND RECURITMENT)

#### 문제

**문제 링크** : https://programmers.co.kr/learn/courses/30/lessons/60062

**문제 설명**

레스토랑을 운영하고 있는 **스카피**는 레스토랑 내부가 너무 낡아 친구들과 함께 직접 리모델링 하기로 했습니다. 레스토랑이 있는 곳은 스노우타운으로 매우 추운 지역이어서 내부 공사를 하는 도중에 주기적으로 외벽의 상태를 점검해야 할 필요가 있습니다.

레스토랑의 구조는 **완전히 동그란 모양**이고 **외벽의 총 둘레는 n미터**이며, 외벽의 몇몇 지점은 추위가 심할 경우 손상될 수도 있는 **취약한 지점들**이 있습니다. 따라서 내부 공사 도중에도 외벽의 취약 지점들이 손상되지 않았는 지, 주기적으로 친구들을 보내서 점검을 하기로 했습니다. 다만, 빠른 공사 진행을 위해 점검 시간을 1시간으로 제한했습니다. 친구들이 1시간 동안 이동할 수 있는 거리는 제각각이기 때문에, 최소한의 친구들을 투입해 취약 지점을 점검하고 나머지 친구들은 내부 공사를 돕도록 하려고 합니다. 편의 상 레스토랑의 정북 방향 지점을 0으로 나타내며, 취약 지점의 위치는 정북 방향 지점으로부터 시계 방향으로 떨어진 거리로 나타냅니다. 또, 친구들은 출발 지점부터 시계, 혹은 반시계 방향으로 외벽을 따라서만 이동합니다.

외벽의 길이 n, 취약 지점의 위치가 담긴 배열 weak, 각 친구가 1시간 동안 이동할 수 있는 거리가 담긴 배열 dist가 매개변수로 주어질 때, 취약 지점을 점검하기 위해 보내야 하는 친구 수의 최소값을 return 하도록 solution 함수를 완성해주세요.

**제한사항**

- n은 1 이상 200 이하인 자연수입니다.
- weak의 길이는 1 이상 15 이하입니다.
  - 서로 다른 두 취약점의 위치가 같은 경우는 주어지지 않습니다.
  - 취약 지점의 위치는 오름차순으로 정렬되어 주어집니다.
  - weak의 원소는 0 이상 n - 1 이하인 정수입니다.
- dist의 길이는 1 이상 8 이하입니다.
  - dist의 원소는 1 이상 100 이하인 자연수입니다.
- 친구들을 모두 투입해도 취약 지점을 전부 점검할 수 없는 경우에는 -1을 return 해주세요.

------

**입출력 예**

| n    | weak             | dist         | result |
| ---- | ---------------- | ------------ | ------ |
| 12   | [1, 5, 6, 10]    | [1, 2, 3, 4] | 2      |
| 12   | [1, 3, 4, 9, 10] | [3, 5, 7]    | 1      |



#### 풀이

처음에 문제를 어떻게 풀어야 하는지 정말 고민했다.

하지만 수학적인 방법으로 마땅히 생각나지 않았고, weak와 dist가 15, 8로 비교적 작은 수였기 때문에, 순열을 사용한 완전탐색을 풀이방향으로 정하였다.



처음에는 weak지점들에 대해 순열을 만들어 모두 시도하였는데 13번 테스트케이스에서 시간초과가 나왔다.



시간초과를 줄이면서 생각해본 몇 가지는 다음과 같다.



1. 방향 Left와 Right는 의미가 없다. (Left에서 가능하면, Right에서도 가능한 경우가 반드시 존재한다.)

   레스토랑이 원형이기 때문에, 결국 돌고 돌게 되어있고, 

   결국 이 문제는 친구가 이동가능한 거리와 각 점간의 거리에 대해 어떻게 배치하냐의 문제이기 때문에

   L R L 과 같이 성공할 수 있다면,  L L L로 성공하는 경우도 반드시 존재한다고 생각하였다.

   그리고 완전탐색으로 모든 경우를 고려할 것이기 때문에,  방향은 Right 한방향으로 고정하였다.

   

2. 투입되는 친구의 순서는 가장 많이 움직일 수 있는 사람부터다. (dist가 큰 값부터)

   

3. 원형이기 때문에, 그냥 레스토랑을 길게 만들어 놓자.

   0 10 20 30 (n = 50) 의 경우 30 -> 0의 처리가 복잡하기 때문에 그냥 (방향은 Right로 고정)

   0 10 20 30 50 60 70 80 으로 만들어놓고 생각하자.



여기서 들어가는 취약지점을 순열로 했을 때 시간 초과가 났기 때문에, 들어가는 친구의 순서를 순열로 고려하자고 생각하였다.



만약 dist = {5, 19, 13, 60}이라면

1명일때 60, 2명일때 60 19 or 19 60, 3명일때 60 19 13|  60 13 19 | 19 60 13....



그렇다면 들어가는 투입지점은 어떻게 정하냐 하면, 그냥 이전 친구가 완료한 시점의 다음으로 하게 했다.

그렇게해서 쭉 실행했을 때, 원하는 범위가 다 방문했다고 표시되면 성공이고, 아니면 실패인 것이다.

또한 처음 시작지점 역시 모든 취약지점에 대해 하였다.



간략하게 정리하자면,

1. 친구의 투입순서를 순열로 만든다 (1명부터 ~ n명까지)
2. 각 순열에 대해 시작지점부터 친구를 한명씩 투입한다. 다음 친구는 이전 친구의 종료지점 다음에 투입한다.
3. 시작지점을 모든 취약지점에 대해 돌아가면서 한다.
4. 중간에 성공하면 바로 탈출



#### 코드

````java
import java.util.Arrays;

class Solutions{
	int[] spandweak;
	int weaknum;
	int distnum;
	int n;
	int answer = -1;
	
	 public int solution(int n, int[] weak, int[] dist) {
		 this.weaknum = weak.length;
		 this.distnum = dist.length;
		 this.n = n;
		 
		 //원 늘리기
		 spandweak = new int[weaknum + weaknum - 1];
		 
		 for(int i =0; i<weaknum; i++) {
			 spandweak[i] = weak[i];
		 }
		 
		 for(int i=weaknum; i<spandweak.length; i++) {
			 spandweak[i] = weak[i-weaknum] + n;
		 }
		 
		 
		//친구는 많이 움직이는 애부터 투입
		 Arrays.sort(dist);
		 
		 
		 for(int i=1; i<=distnum; i++) {
			 int[] inputfriend = new int[i];
			 
			 //넣을 친구는 큰 수부터
			 for(int j=0; j<i; j++) {
				 inputfriend[j] = dist[distnum-1-j];
			 }
			 
			 PermutationSwap(inputfriend, 0, i, i);
			 if(answer > -1) break;
		 }
		 return answer;
	 }
	
    //순열 (친구 수)
	public void PermutationSwap(int[] arr, int index, int n, int k) {
		if(answer > -1) return;
		
		if (index == k) {
			if(Check(arr, k)) answer = k;
			return;
		}
		
		for (int i=index; i<n; i++) {
			
			int temp = arr[index];
			arr[index] = arr[i];
			arr[i] = temp;
	    	
	    	PermutationSwap(arr, index + 1, n, k);
	    	
	    	//다시 원위치
	    	temp = arr[index];
			arr[index] = arr[i];
			arr[i] = temp;
	    	
		}
	}
	
	public boolean Check(int[] inputfriend, int friendnum) {
		
		int[] check = new int[weaknum*2-1];
		int startindex;
		int index;
		int length;
		
		//print(inputfriend);
		
		
		for(int i=0; i<weaknum; i++) {
			startindex = i; //시작점
			index = i;
			
			for(int j=0; j<friendnum; j++) {
				
				length = inputfriend[j];
				
				//시작위치 체크
				check[index] = 1;
				
				while(length >= 0) {
					//마지막이면 종료
					if(index >= startindex + weaknum -1) break;
					length = length  + spandweak[index] - spandweak[index+1];
					if(length >= 0) {
						check[index+1] = 1;
					}
                    //다음
					index++;
				}
			}
			//print(check);
            //시작점 ~ 범위까지 다 체크되어있으면 성공
			if(checkList(check, startindex, startindex+weaknum)) return true;
			initList(check,startindex,startindex+weaknum);
		}
		return false;
		
		
	}
	
	public boolean checkList(int[] checked, int start, int end) {
		for(int i=start; i<end; i++) {
			if(checked[i] != 1) {
				return false;
			}
		}
		return true;
	}
	
	public void initList(int[] checked, int start, int end) {
		for(int i=start; i<end; i++) {
			checked[i] = 0;
		}
	}
	
	public void print(int[] arr) {
		for(int i=0; i<arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println("\n");
	}	
}
````



#### 결과