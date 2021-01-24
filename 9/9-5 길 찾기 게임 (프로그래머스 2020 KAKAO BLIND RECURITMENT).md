## 9-5 길 찾기 게임 (프로그래머스 2020 KAKAO BLIND RECURITMENT)

#### 문제

**문제 링크** : https://programmers.co.kr/learn/courses/30/lessons/42892

**문제 설명**

전무로 승진한 라이언은 기분이 너무 좋아 프렌즈를 이끌고 특별 휴가를 가기로 했다.
내친김에 여행 계획까지 구상하던 라이언은 재미있는 게임을 생각해냈고 역시 전무로 승진할만한 인재라고 스스로에게 감탄했다.

라이언이 구상한(그리고 아마도 라이언만 즐거울만한) 게임은, 카카오 프렌즈를 두 팀으로 나누고, 각 팀이 같은 곳을 다른 순서로 방문하도록 해서 먼저 순회를 마친 팀이 승리하는 것이다.

그냥 지도를 주고 게임을 시작하면 재미가 덜해지므로, 라이언은 방문할 곳의 2차원 좌표 값을 구하고 각 장소를 이진트리의 노드가 되도록 구성한 후, 순회 방법을 힌트로 주어 각 팀이 스스로 경로를 찾도록 할 계획이다.

라이언은 아래와 같은 특별한 규칙으로 트리 노드들을 구성한다.

- 트리를 구성하는 모든 노드의 x, y 좌표 값은 정수이다.
- 모든 노드는 서로 다른 x값을 가진다.
- 같은 레벨(level)에 있는 노드는 같은 y 좌표를 가진다.
- 자식 노드의 y 값은 항상 부모 노드보다 작다.
- 임의의 노드 V의 왼쪽 서브 트리(left subtree)에 있는 모든 노드의 x값은 V의 x값보다 작다.
- 임의의 노드 V의 오른쪽 서브 트리(right subtree)에 있는 모든 노드의 x값은 V의 x값보다 크다.

아래 예시를 확인해보자.

라이언의 규칙에 맞게 이진트리의 노드만 좌표 평면에 그리면 다음과 같다. (이진트리의 각 노드에는 1부터 N까지 순서대로 번호가 붙어있다.)

![img](https://blog.kakaocdn.net/dn/bEqnl1/btqUtV51tzB/MagYBnlOIcFZ7jwfL9l8Xk/img.jpg)

이제, 노드를 잇는 간선(edge)을 모두 그리면 아래와 같은 모양이 된다.

![img](https://blog.kakaocdn.net/dn/cE3NIv/btqUtXJuWoK/Om2A41r2ebiRdqAGjyvaOk/img.jpg)

위 이진트리에서 전위 순회(preorder), 후위 순회(postorder)를 한 결과는 다음과 같고, 이것은 각 팀이 방문해야 할 순서를 의미한다.

- 전위 순회 : 7, 4, 6, 9, 1, 8, 5, 2, 3
- 후위 순회 : 9, 6, 5, 8, 1, 4, 3, 2, 7

다행히 두 팀 모두 머리를 모아 분석한 끝에 라이언의 의도를 간신히 알아차렸다.

그러나 여전히 문제는 남아있다. 노드의 수가 예시처럼 적다면 쉽게 해결할 수 있겠지만, 예상대로 라이언은 그렇게 할 생각이 전혀 없었다.

이제 당신이 나설 때가 되었다.

곤경에 빠진 카카오 프렌즈를 위해 이진트리를 구성하는 노드들의 좌표가 담긴 배열 nodeinfo가 매개변수로 주어질 때,
노드들로 구성된 이진트리를 전위 순회, 후위 순회한 결과를 2차원 배열에 순서대로 담아 return 하도록 solution 함수를 완성하자.



**제한사항**

- nodeinfo는 이진트리를 구성하는 각 노드의 좌표가 1번 노드부터 순서대로 들어있는 2차원 배열이다.
- nodeinfo의 길이는 `1` 이상 `10,000` 이하이다.
- nodeinfo[i] 는 i + 1번 노드의 좌표이며, [x축 좌표, y축 좌표] 순으로 들어있다.
- 모든 노드의 좌표 값은 `0` 이상 `100,000` 이하인 정수이다.
- 트리의 깊이가 `1,000` 이하인 경우만 입력으로 주어진다.
- 모든 노드의 좌표는 문제에 주어진 규칙을 따르며, 잘못된 노드 위치가 주어지는 경우는 없다.

------

**입출력 예**

| nodeinfo                                                  | result                                    |
| --------------------------------------------------------- | ----------------------------------------- |
| [[5,3],[11,5],[13,3],[3,5],[6,1],[1,3],[8,6],[7,2],[2,2]] | [[7,4,6,9,1,8,5,2,3],[9,6,5,8,1,4,3,2,7]] |



#### 풀이

문제를 풀이함에 있어서 트리를 직접 만드는 방법이 제일 간편하다고 생각했다.

이때 트리를 만들 때, 배열로 인덱스로 하기에는, 깊이가 1000까지 있기 때문에 연결리스트 형식으로 해야한다.



먼저 트리를 위해 노드를 정의하였다. 노드는 왼/오 자식 노드, 좌표, 번호를 가지고 있다.

트리의 루트를 정하기 위해 각 좌표들을 정렬하고,  해시맵을 사용하여 각각의 번호를 저장해두었다.

이후 문제의 조건처럼, 같은 레벨(y)이 아닌 경우 x좌표로 왼, 오를 결정하는 방식에 따라 재귀적으로 트리를 구성하도록 하였다.



이후 전위, 후위순회 역시 재귀로 구현하여 정답을 도출하였다.



#### 코드

````java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
class Solution {
	
	int[][] nodeinfo;
	int size;
	
	HashMap<int[], Integer> indexMap;
	
	//for tree
	Node root;
	ArrayList<Integer> pre = new ArrayList<>();
	ArrayList<Integer> post = new ArrayList<>();
    
     //node
    public class Node{
    	int loc[];
    	//for answer
    	int idx;
    	
    	Node left;
    	Node right;
    	
    	public Node(int[] a, int i) {
    		this.loc = a;
    		this.idx = i;
    	}
    }
	
    public int[][] solution(int[][] nodeinfo) {
        
        //initTree
        size = nodeinfo.length;
        this.nodeinfo = nodeinfo;
        int[][] answer = new int[2][size];
        
        //reserve the index
        indexMap = new HashMap<int[], Integer>();
        for(int i=0; i<nodeinfo.length; i++) {
        	indexMap.put(nodeinfo[i], i+1);
        }
        

        //sort the nodes
        Arrays.sort(nodeinfo, (o1, o2) -> {
        	if(o1[1] == o2[1]) {
        		return Integer.compare(o1[0], o2[0]);
        	}else {
        		return Integer.compare(o1[1], o2[1]);
        	}
        });
        
        makeTree();
        preorder(root);
        postorder(root);
        
         for(int i=0; i<size; i++) {
        	answer[0][i] = pre.get(i);
        	answer[1][i] = post.get(i);
        }
        
        //answer[0] = Arrays.stream(pre.toArray(new Integer[size])).mapToInt(Integer::intValue).toArray();
        //answer[1] = Arrays.stream(post.toArray(new Integer[size])).mapToInt(Integer::intValue).toArray();
        
        return answer;
    }
    
    public void addNode(Node n, Node parent) {
        //left
		if(parent.loc[0] > n.loc[0]) {
			if(parent.left == null) {
				parent.left = n;
				return;
			}
			else { 
				addNode(n, parent.left);
			}
		}
		//right
		else if(parent.loc[0] < n.loc[0]){
			if(parent.right == null) {
				parent.right = n;
				return;
			}
			else {
				addNode(n, parent.right);
			}	
		}
	}
    
    public void makeTree(){
    	root = new Node(nodeinfo[size-1], indexMap.get(nodeinfo[size-1]));
    	
    	for(int i=size-2; i>=0; i--) {
    		addNode(new Node(nodeinfo[i], indexMap.get(nodeinfo[i])), root);
    	}
    }
    
    public void preorder(Node r) {
    	if(r != null) {
    		pre.add(r.idx);
    		preorder(r.left);
    		preorder(r.right);
    	}
    }
    
    public void postorder(Node r) {
    	if(r!= null) {
    		postorder(r.left);
    		postorder(r.right);
    		post.add(r.idx);
    	}
    }
    
   
}
````



#### 결과

![img](https://blog.kakaocdn.net/dn/4t5en/btqUwxwzfau/zAwSMxQy8bIp1eETTVmwqK/img.png)