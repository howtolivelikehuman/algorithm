## 9-2 후보키 (프로그래머스)

프렌즈대학교 컴퓨터공학과 조교인 제이지는 네오 학과장님의 지시로, 학생들의 인적사항을 정리하는 업무를 담당하게 되었다.

그의 학부 시절 프로그래밍 경험을 되살려, 모든 인적사항을 데이터베이스에 넣기로 하였고, 이를 위해 정리를 하던 중에 후보키(Candidate Key)에 대한 고민이 필요하게 되었다.

후보키에 대한 내용이 잘 기억나지 않던 제이지는, 정확한 내용을 파악하기 위해 데이터베이스 관련 서적을 확인하여 아래와 같은 내용을 확인하였다.

- 관계 데이터베이스에서 릴레이션(Relation)의 튜플(Tuple)을 유일하게 식별할 수 있는 속성(Attribute) 또는 속성의 집합 중, 다음 두 성질을 만족하는 것을 후보 키(Candidate Key)라고 한다.
  - 유일성(uniqueness) : 릴레이션에 있는 모든 튜플에 대해 유일하게 식별되어야 한다.
  - 최소성(minimality) : 유일성을 가진 키를 구성하는 속성(Attribute) 중 하나라도 제외하는 경우 유일성이 깨지는 것을 의미한다. 즉, 릴레이션의 모든 튜플을 유일하게 식별하는 데 꼭 필요한 속성들로만 구성되어야 한다.

제이지를 위해, 아래와 같은 학생들의 인적사항이 주어졌을 때, 후보 키의 최대 개수를 구하라.

![cand_key1.png](https://grepp-programmers.s3.amazonaws.com/files/production/f1a3a40ede/005eb91e-58e5-4109-9567-deb5e94462e3.jpg)

위의 예를 설명하면, 학생의 인적사항 릴레이션에서 모든 학생은 각자 유일한 학번을 가지고 있다. 따라서 학번은 릴레이션의 후보 키가 될 수 있다.
그다음 이름에 대해서는 같은 이름(apeach)을 사용하는 학생이 있기 때문에, 이름은 후보 키가 될 수 없다. 그러나, 만약 [이름, 전공]을 함께 사용한다면 릴레이션의 모든 튜플을 유일하게 식별 가능하므로 후보 키가 될 수 있게 된다.
물론 [이름, 전공, 학년]을 함께 사용해도 릴레이션의 모든 튜플을 유일하게 식별할 수 있지만, 최소성을 만족하지 못하기 때문에 후보 키가 될 수 없다.
따라서, 위의 학생 인적사항의 후보키는 학번, [이름, 전공] 두 개가 된다.

릴레이션을 나타내는 문자열 배열 relation이 매개변수로 주어질 때, 이 릴레이션에서 후보 키의 개수를 return 하도록 solution 함수를 완성하라.

##### 제한사항

- relation은 2차원 문자열 배열이다.
- relation의 컬럼(column)의 길이는 `1` 이상 `8` 이하이며, 각각의 컬럼은 릴레이션의 속성을 나타낸다.
- relation의 로우(row)의 길이는 `1` 이상 `20` 이하이며, 각각의 로우는 릴레이션의 튜플을 나타낸다.
- relation의 모든 문자열의 길이는 `1` 이상 `8` 이하이며, 알파벳 소문자와 숫자로만 이루어져 있다.
- relation의 모든 튜플은 유일하게 식별 가능하다.(즉, 중복되는 튜플은 없다.)

##### 입출력 예

| relation                                                     | result |
| ------------------------------------------------------------ | ------ |
| `[["100","ryan","music","2"],["200","apeach","math","2"],["300","tube","computer","3"],["400","con","computer","4"],["500","muzi","music","3"],["600","apeach","music","2"]]` | 2      |

#### 풀이

문제를 생각나는대로 순서대로 풀었다.

전체 배열이 20*8크기에, 문자열의 길이가 8이하이므로 그렇게 큰 시간이 걸릴 것 같지 않았다.



우선 유일성을 체크하는 FindUnique 함수를 만들었다.

해시맵을 사용하여 겹치는게 있는지 체크하는데, 이때 (round = 몇개의 속성), (index = 속성의 열 번호들)을 입력받아 여러개의 속성으로도 비교할 수 있게 하였다.

> round =2, index = 1,2
>
> "ryan music", "apeach math", "tube computer"... 끼리 비교



그리고 후보키를 찾음에 있어서, 실질적으로 문제가 되는 부분이 혼자서 제 역할을 다하지 못하는 속성들이라 생각하여,

각 속성 1개씩만으로 후보키를 찾은 다음, 만족하지 못한 열들을 unavailable에 저장해두었다.



이제 이 unavailable에 있는 속성들을 대상으로 2개..n개까지의 조합을 만들어 일일히 검사해보면 된다.

따라서 조합을 만드는 Combination 함수를 만들었다.



여기서 고려해야 할 부분이 최소성인데 딱히 좋은 방법이 떠오르지 않았다.

> 1 2 = 가능, 1 2 3 = 가능해도 최소성 만족 x
>
> 여기서 1, 2를 배제하자니
>
> 1 2 = 가능, 1 4 5 -> 가능할수도

따라서 그냥 가능한 경우를 successList에 저장하고, 매 조합마다 exist 메소드를 사용하여 successList에 있는 성공조합과 비교하도록 하였다.



#### 코드

````java
import java.util.ArrayList;
import java.util.HashMap;

class Solution {
    String[][] relation;
    int column=0;
    int rows =0;
    int answer=0;

    public int solution(String[][] relation) {
        ArrayList<Integer> unavailable = new ArrayList<>();
        ArrayList<int[]> successList = new ArrayList<>();

        this.relation = relation;
        column = relation[0].length;
        rows = relation.length;

        
        //개별로 불가능한 속성 찾기
        int index[] = new int[1];
        for(int i=0; i<column; i++){
            index[0] = i;
            if(!FindUnique(1,index)){
                unavailable.add(i);
            };
        }

        int round = 2;
        int size = unavailable.size();
        //2개~ size개까지 조합
        while (round <= size){
            int[] output = new int[round];
            Combination(successList, unavailable, 0, round, 0, output);
            round++;
        }

        System.out.println(answer);
        return answer;
    }

    public boolean FindUnique(int round, int index[]){
        HashMap<String, Integer> hashMap = new HashMap<String, Integer>();
        for(int i=0; i<rows; i++){
            String key = "";
            //키 조합하기
            for(int j=0; j<round; j++){ 
                 key += " "+relation[i][index[j]];
            }
            //해시맵에서 키 있는지 확인
            if(hashMap.get(key) == null){ 
                hashMap.put(key,0);
            }else{
              return false;
            }
        }
        answer++;
        return true;
    }
    
	//조합
    public void Combination (ArrayList<int[]> success, ArrayList<Integer> lists, int index, int round,  int left, int output[]){
        if(round == 0){
            //존재하는거면 끝
            for(int i=0; i<success.size(); i++){
                if(exist(success.get(i), output)){
                    return;
                }
            }
            //가능한 조합이면 successList에 추가
            if(FindUnique(output.length,output)){
                success.add(output.clone());
            }
            return;
        }
        if(left == lists.size()) return;
        
        output[index] = lists.get(left);
        Combination(success, lists, index+1, round-1, left+1, output);
        Combination(success, lists, index, round, left+1,output);
    }
	//a가 b에 들어있는지 확인
    public boolean exist(int[] a, int[] b){
        int count = 0;
        for(int i=0; i<a.length; i++){
            count = 0;
            for(int j=0; j<b.length; j++){
                if(a[i] == b[j]){
                    count++;
                    break;
                }
            }//없으면 바로 false
            if(count == 0) return false;
        }//끝까지 다 있으면 true
        return true;
    }
}
````

#### 결과

![img](https://blog.kakaocdn.net/dn/dhMGKG/btqRTSDLg1U/JR0OKUYBQBs8KvVsvoXE30/img.png)

