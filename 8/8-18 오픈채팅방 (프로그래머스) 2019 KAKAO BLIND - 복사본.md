## 8-16 괄호 변환 (프로그래머스) [JAVA] 2020 KAKAO BLIND RECRUITMENT

#### 문제

카카오톡 오픈채팅방에서는 친구가 아닌 사람들과 대화를 할 수 있는데, 본래 닉네임이 아닌 가상의 닉네임을 사용하여 채팅방에 들어갈 수 있다.

신입사원인 김크루는 카카오톡 오픈 채팅방을 개설한 사람을 위해, 다양한 사람들이 들어오고, 나가는 것을 지켜볼 수 있는 관리자창을 만들기로 했다. 채팅방에 누군가 들어오면 다음 메시지가 출력된다.

[닉네임]님이 들어왔습니다.

채팅방에서 누군가 나가면 다음 메시지가 출력된다.

[닉네임]님이 나갔습니다.

채팅방에서 닉네임을 변경하는 방법은 다음과 같이 두 가지이다.

- 채팅방을 나간 후, 새로운 닉네임으로 다시 들어간다.
- 채팅방에서 닉네임을 변경한다.

닉네임을 변경할 때는 기존에 채팅방에 출력되어 있던 메시지의 닉네임도 전부 변경된다.

예를 들어, 채팅방에 Muzi와 Prodo라는 닉네임을 사용하는 사람이 순서대로 들어오면 채팅방에는 다음과 같이 메시지가 출력된다.

Muzi님이 들어왔습니다.
Prodo님이 들어왔습니다.

채팅방에 있던 사람이 나가면 채팅방에는 다음과 같이 메시지가 남는다.

Muzi님이 들어왔습니다.
Prodo님이 들어왔습니다.
Muzi님이 나갔습니다.

Muzi가 나간후 다시 들어올 때, Prodo 라는 닉네임으로 들어올 경우 기존에 채팅방에 남아있던 Muzi도 Prodo로 다음과 같이 변경된다.

Prodo님이 들어왔습니다.
Prodo님이 들어왔습니다.
Prodo님이 나갔습니다.
Prodo님이 들어왔습니다.

채팅방은 중복 닉네임을 허용하기 때문에, 현재 채팅방에는 Prodo라는 닉네임을 사용하는 사람이 두 명이 있다. 이제, 채팅방에 두 번째로 들어왔던 Prodo가 Ryan으로 닉네임을 변경하면 채팅방 메시지는 다음과 같이 변경된다.

Prodo님이 들어왔습니다.
Ryan님이 들어왔습니다.
Prodo님이 나갔습니다.
Prodo님이 들어왔습니다.

채팅방에 들어오고 나가거나, 닉네임을 변경한 기록이 담긴 문자열 배열 record가 매개변수로 주어질 때, 모든 기록이 처리된 후, 최종적으로 방을 개설한 사람이 보게 되는 메시지를 문자열 배열 형태로 return 하도록 solution 함수를 완성하라.

##### 제한사항

- record는 다음과 같은 문자열이 담긴 배열이며, 길이는 `1` 이상 `100,000` 이하이다.
- 다음은 record에 담긴 문자열에 대한 설명이다.
  - 모든 유저는 [유저 아이디]로 구분한다.
  - [유저 아이디] 사용자가 [닉네임]으로 채팅방에 입장 - Enter [유저 아이디] [닉네임] (ex. Enter uid1234 Muzi)
  - [유저 아이디] 사용자가 채팅방에서 퇴장 - Leave [유저 아이디] (ex. Leave uid1234)
  - [유저 아이디] 사용자가 닉네임을 [닉네임]으로 변경 - Change [유저 아이디] [닉네임] (ex. Change uid1234 Muzi)
  - 첫 단어는 Enter, Leave, Change 중 하나이다.
  - 각 단어는 공백으로 구분되어 있으며, 알파벳 대문자, 소문자, 숫자로만 이루어져있다.
  - 유저 아이디와 닉네임은 알파벳 대문자, 소문자를 구별한다.
  - 유저 아이디와 닉네임의 길이는 `1` 이상 `10` 이하이다.
  - 채팅방에서 나간 유저가 닉네임을 변경하는 등 잘못 된 입력은 주어지지 않는다.

##### 입출력 예

| record                                                       | result                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `["Enter uid1234 Muzi", "Enter uid4567 Prodo","Leave uid1234","Enter uid1234 Prodo","Change uid4567 Ryan"]` | `["Prodo님이 들어왔습니다.", "Ryan님이 들어왔습니다.", "Prodo님이 나갔습니다.", "Prodo님이 들어왔습니다."]` |



#### 풀이

제한사항에 보면 말도안되는 일 (먼저 나갔다가 들어오기 등)은 벌어지지 않으므로, 순차적으로 작업을 분류하기만 하면 된다고 생각했다.

유니크한 값인 아이디와 이름의 매칭이 중요하다고 생각했고 맨 처음 1회 반복에서는 해시맵을 사용해서 아이디 - 이름을 매칭하였다. 

이때 이름이 변경되는 경우가 Change 호출과, Enter시 새로운 이름으로 올 경우이므로 두 경우만 체크해주었고, 

Change는 정답에 기록되지 않으므로 null처리를 해서 비교를 편하게? 했다.

Enter와 Leave 역시 해시맵으로 미리 메시지를 저장해두었다.

두번째 반복에서 이제 순차적으로 해시맵을 통해 번역해여 정답에 기록하였다.



#### 코드

````java
import java.util.HashMap;
class Solution {
    public String[] solution(String[] record) {
        String[] answer;
        StringBuilder ans = new StringBuilder();
        HashMap<String, String> ID_Name = new HashMap<String, String>();
        HashMap<String, String> Action_Korean = new HashMap<String, String>();

        Action_Korean.put("Enter", "님이 들어왔습니다.");
        Action_Korean.put("Leave", "님이 나갔습니다.");

        int index = 0;
        for(int i=0; i<record.length; i++) {
            String[] rec = record[i].split(" ");

            if(rec[0].equals("Enter")) {
                //id와 이름 연결
                if(!ID_Name.containsKey(rec[1])) {
                    ID_Name.put(rec[1], rec[2]);
                }else {
                    ID_Name.replace(rec[1], ID_Name.get(rec[1]), rec[2]);
                }
            }
            //닉네임 바꿔야 할때
            else if(rec[0].equals("Change")) {
                ID_Name.replace(rec[1], ID_Name.get(rec[1]), rec[2]);
                record[i] = null;
                index++;
            }
        }
        index = record.length - index;

        answer = new String[index];

        for(int i=record.length-1; i>=0; i--) {
            if(record[i] != null) {
                String[] rec = record[i].split(" ");
                answer[--index] = ID_Name.get(rec[1])+Action_Korean.get(rec[0]);
                //System.out.println(answer[index]);
            }
        }
        return answer;
    }
}
````

#### 결과

````
테스트 1 〉	통과 (2.18ms, 52.6MB)
테스트 2 〉	통과 (1.76ms, 53.3MB)
테스트 3 〉	통과 (2.14ms, 52MB)
테스트 4 〉	통과 (2.08ms, 52.5MB)
테스트 5 〉	통과 (9.55ms, 55.5MB)
테스트 6 〉	통과 (9.74ms, 54.1MB)
테스트 7 〉	통과 (8.15ms, 54.4MB)
테스트 8 〉	통과 (10.70ms, 54.3MB)
테스트 9 〉	통과 (10.96ms, 54.4MB)
테스트 10 〉	통과 (8.43ms, 54.3MB)
테스트 11 〉	통과 (7.77ms, 54MB)
테스트 12 〉	통과 (6.37ms, 53.5MB)
테스트 13 〉	통과 (9.52ms, 54.1MB)
테스트 14 〉	통과 (9.75ms, 54.5MB)
테스트 15 〉	통과 (1.72ms, 51.6MB)
테스트 16 〉	통과 (2.01ms, 53.1MB)
테스트 17 〉	통과 (2.66ms, 52.8MB)
테스트 18 〉	통과 (5.85ms, 52.2MB)
테스트 19 〉	통과 (10.51ms, 54.6MB)
테스트 20 〉	통과 (10.04ms, 53.7MB)
테스트 21 〉	통과 (8.40ms, 53.8MB)
테스트 22 〉	통과 (8.59ms, 54.4MB)
테스트 23 〉	통과 (11.56ms, 54.7MB)
테스트 24 〉	통과 (11.29ms, 54.1MB)
테스트 25 〉	통과 (239.14ms, 148MB)
테스트 26 〉	통과 (254.09ms, 151MB)
테스트 27 〉	통과 (274.99ms, 156MB)
테스트 28 〉	통과 (305.80ms, 168MB)
테스트 29 〉	통과 (324.53ms, 170MB)
테스트 30 〉	통과 (245.39ms, 142MB)
테스트 31 〉	통과 (257.20ms, 155MB)
테스트 32 〉	통과 (210.83ms, 142MB)
````