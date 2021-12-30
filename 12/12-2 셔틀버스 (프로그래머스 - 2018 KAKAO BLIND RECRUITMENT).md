

## 12-2 셔틀버스 (프로그래머스 - 2018 KAKAO BLIND RECRUITMENT)

#### 문제

**문제 링크** : https://programmers.co.kr/learn/courses/30/lessons/17678

**문제**

카카오에서는 무료 셔틀버스를 운행하기 때문에 판교역에서 편하게 사무실로 올 수 있다. 카카오의 직원은 서로를 '크루'라고 부르는데, 아침마다 많은 크루들이 이 셔틀을 이용하여 출근한다.

이 문제에서는 편의를 위해 셔틀은 다음과 같은 규칙으로 운행한다고 가정하자.

- 셔틀은 `09:00`부터 총 `n`회 `t`분 간격으로 역에 도착하며, 하나의 셔틀에는 최대 `m`명의 승객이 탈 수 있다.
- 셔틀은 도착했을 때 도착한 순간에 대기열에 선 크루까지 포함해서 대기 순서대로 태우고 바로 출발한다. 예를 들어 `09:00`에 도착한 셔틀은 자리가 있다면 `09:00`에 줄을 선 크루도 탈 수 있다.

일찍 나와서 셔틀을 기다리는 것이 귀찮았던 콘은, 일주일간의 집요한 관찰 끝에 어떤 크루가 몇 시에 셔틀 대기열에 도착하는지 알아냈다. 콘이 셔틀을 타고 사무실로 갈 수 있는 도착 시각 중 제일 늦은 시각을 구하여라.

단, 콘은 게으르기 때문에 같은 시각에 도착한 크루 중 대기열에서 제일 뒤에 선다. 또한, 모든 크루는 잠을 자야 하므로 `23:59`에 집에 돌아간다. 따라서 어떤 크루도 다음날 셔틀을 타는 일은 없다.

**입력형식**

셔틀 운행 횟수 `n`, 셔틀 운행 간격 `t`, 한 셔틀에 탈 수 있는 최대 크루 수 `m`, 크루가 대기열에 도착하는 시각을 모은 배열 `timetable`이 입력으로 주어진다.

- 0 ＜ `n` ≦ 10
- 0 ＜ `t` ≦ 60
- 0 ＜ `m` ≦ 45
- `timetable`은 최소 길이 1이고 최대 길이 2000인 배열로, 하루 동안 크루가 대기열에 도착하는 시각이 `HH:MM` 형식으로 이루어져 있다.
- 크루의 도착 시각 `HH:MM`은 `00:01`에서 `23:59` 사이이다.

**출력형식**

- 콘이 무사히 셔틀을 타고 사무실로 갈 수 있는 제일 늦은 도착 시각을 출력한다. 도착 시각은 `HH:MM` 형식이며, `00:00`에서 `23:59` 사이의 값이 될 수 있다.



#### 풀이

시간의 전체 범위가 0시부터 ~ 23시59분 ->  1440분이기 때문에 모든 시간 단위를 배열로 선언하고, 입력받은 timetable에 따라 각 시간마다 사람을 세팅했다.

이후 0시부터 시작해서 차가 오는 9시까지, 그 이후는 배차시간마다 마지막차 전까지 인원을 계산해준다.

카운터를 사용해서, 현재 몇분에 온 사람까지 탔는지를 계속 유지한다.

마지막 차에 대해서 역시 동일하게 진행하는데, 이때 막차도 사람이 가득 찬다면 -> 마지막 인덱스보다 1분 빨리 (같은 분에서는 콘이 제일 뒤이므로)하고

차지 않는다면 마지막 인덱스에 줄을 스도록 하면 된다.



#### 코드 (JAVA)

```java
import java.io.*;
public class Solution {
    int start = 540;
    int[] timeslot = new int[1440];

    public String solution(int n, int t, int m, String[] timetable) {

        //시간표에 사람 채워넣기
        for (int i=0; i<timetable.length; i++){
            timeslot[convertMin(timetable[i])]++;
        }
        //직전까지 시간 이동
        int index = 0;
        int number;

        for(int i=0; i<n-1; i++){
            number = m;

            for(int time = index; time <= start; time++){
                while(timeslot[time] > 0){
                    timeslot[time]--;
                    number--;
                    if(number == 0) break;
                }
                index = time;
                if(number == 0) break;
            }
            start = start + t;
        }

        //막차
        number = m;
        for(int time = index; time <= start; time++){
            while(timeslot[time] > 0){
                timeslot[time]--;
                number--;
                if(number == 0) break;
            }
            index = time;
            if(number == 0) break;
        }

        if(number <= 0){
            index = index-1;
        }

        return convertHour(index);
    }

    public int convertMin(String time){
        String[] c = time.split(":");
        return Integer.parseInt(c[0])*60 + Integer.parseInt(c[1]);
    }

    public String convertHour(int min){
        int hour = min/60;
        String hourS = hour > 9 ? String.valueOf(hour) : 0+String.valueOf(hour);
        min = min%60;
        String minS = min > 9 ? String.valueOf(min) : 0+String.valueOf(min);

        return hourS+":"+minS;
    }
}
```

#### 결과

![image info](./img/result-12-2.png)

