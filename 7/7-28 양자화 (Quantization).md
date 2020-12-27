## 7-28 양자화 (Quantization)

|                            문제ID                            |  시간제한  | 메모리제한  |                           제출횟수                           |                       정답 횟수(비율)                        |
| :----------------------------------------------------------: | :--------: | :---------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| [QUANTIZE](https://www.algospot.com/judge/problem/read/QUANTIZE) | **3000**ms | **65536**kb | [**5597**](https://www.algospot.com/judge/submission/recent/?problem=QUANTIZE) | [**1825** (**32**%)](https://www.algospot.com/judge/problem/stat/QUANTIZE/) |

#### 문제

Quantization (양자화) 과정은, 더 넓은 범위를 갖는 값들을 작은 범위를 갖는 값들로 근사해 표현함으로써 자료를 손실 압축하는 과정을 말한다. 예를 들어 16비트 JPG 파일을 4컬러 GIF 파일로 변환하는 것은 RGB 색 공간의 색들을 4컬러 중의 하나로 양자화하는 것이고, 키가 161, 164, 170, 178 인 학생 넷을 '160대 둘, 170대 둘' 이라고 축약해 표현하는 것 또한 양자화라고 할 수 있다.

1000 이하의 자연수들로 구성된 수열을 최대 S종류 의 값만을 사용하도록 양자화하고 싶다. **이 때 양자화된 숫자는 원래 수열에 없는 숫자일 수도 있다.** 양자화를 하는 방법은 여러 가지가 있다. 수열 1 2 3 4 5 6 7 8 9 10 을 2개의 숫자만을 써서 표현하려면, 3 3 3 3 3 7 7 7 7 7 과 같이 할 수도 있고, 1 1 1 1 1 10 10 10 10 10 으로 할 수도 있다. 우리는 이 중, 각 숫자별 오차 제곱의 합을 최소화하는 양자화 결과를 알고 싶다.

예를 들어, 수열 1 2 3 4 5 를 1 1 3 3 3 으로 양자화하면 오차 제곱의 합은 0+1+0+1+4=6 이 되고, 2 2 2 4 4 로 양자화하면 오차 제곱의 합은 1+0+1+0+1=3 이 된다.

수열과 S 가 주어질 때, 가능한 오차 제곱의 합의 최소값을 구하는 프로그램을 작성하시오.

#### 입력

입력의 첫 줄에는 테스트 케이스의 수 C (1 <= C <= 50) 가 주어진다. 각 테스트 케이스의 첫 줄에는 수열의 길이 N (1 <= N <= 100), 사용할 숫자의 수 S (1 <= S <= 10) 이 주어진다. 그 다음 줄에 N개의 정수로 수열의 숫자들이 주어진다. 수열의 모든 수는 1000 이하의 자연수이다.

#### 출력

각 테스트 케이스마다, 주어진 수열을 최대 S 개의 수로 양자화할 때 오차 제곱의 합의 최소값을 출력한다.

#### 풀이

2가지 포인트가 있다고 한다.

1. 제곱의 오차를 빠르게 구하는 것
2. 구간을 잡는 것



제곱의 오차를 구하는 것은 지정한 구간 내의 평균값(반올림)이 최소라고 한다. 이때, 식을 정리할 수 있다고 한다.

$\sum_{i = a}^{b}({x-m})^2 = \sum_{i = a}^{b}x^2 - 2m\sum_{i = a}^{b}x + m^2(b-a+1) $ 

어차피 우리는 모든 부분합을 brute force로 해볼것이기 때문에, 식의 처음에 $\sum_{i = a}^{b}x^2$ 과 $\sum_{i = a}^{b}x$ 만 미리 저장해두면 $O(1)$로 평균을 구할 수 있다.



구간을 잡는 방법은 아래와 같이 표현할 수 있다.

$A_n(start,count) = min(An(start,count), Sum(start,start+n-1) + An(start+n,count-1))$



처음에 이렇게 시도하고,

> ```
> [0~0] + [1...10]
>         [1...10] == [1~1], [1~2], [1~3], [1~4], [1~5], [1~6], [1~7], [1~8], [1~9], [1~10]
> ```

두번째는 이렇게

> ```
> [0~0] [1~1] + [2...10]
>               [2...10] == [2~2], [2~3], [2~4], [2~5], [2~6], [2~7], [2~8], [2~9], [2~10]
> ```

이런 방식으로 쭉 나누는 것이다.

아니면, 조합을 사용해서 나눌수도 있다.



시간복잡도는 부분문제의 수 n*s 에 계산하는데 걸린 n을 곱한 $O(n^2s)$이다.

#### 코드

````java
import java.util.Arrays;
public class Main {
	public static StringBuilder sb = new StringBuilder();
    public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
    public static int N, S;
    public static int[] numbers;
    public static int[] pSum;
    public static int[] pSeqSum;
    public static int ans;
    public static int cache[][];
    public static void main(String[] args)throws Exception {
    	
    	int C = Integer.parseInt(br.readLine().trim());
    	
    	for( ; C > 0; C--) {
    		
    		String stl[] = br.readLine().trim().split(" ");
    		N = Integer.parseInt(stl[0]);
    		S = Integer.parseInt(stl[1]);
    		
    		numbers = new int[N];
    		pSum = new int[N];
    	    pSeqSum = new int[N];
    	    cache = new int[N][N];
    	    
    		stl = br.readLine().trim().split(" ");  
    		
    		if(S>=N) {
    			ans = 0;
    			sb.append(ans).append("\n");
    			continue;
    		}
    		
    		for(int i=0; i<N; i++) {
    			numbers[i] = Integer.parseInt(stl[i]);
    			for(int x=0; x< N; x++) {
    				cache[i][x] = -1;
    			}
    		}
    		
    		Arrays.sort(numbers);
    		
    	    pSum[0] = numbers[0];
    	    pSeqSum[0] = numbers[0]*numbers[0];
    	    //미리 부분합들 계산
    	    for(int i=1; i<N; i++) {
    	    	pSum[i] = pSum[i-1]+numbers[i];
    	    	pSeqSum[i] = pSeqSum[i-1] + numbers[i]*numbers[i];
    	    }
    	    
    		ans = Part(0,S);
    		sb.append(ans).append("\n");
    	}
    	System.out.println(sb.toString());
    }
    
    public static int Part(int start, int count) {
    	//숫자가 다 양자화 됨
    	if(start == N) {
    		return 0;
    	}
    	//더이상 묶을 수 없음
    	if(count == 0) {
    		return 987654321;
    	}
    	
    	int minSum = cache[start][count];
    	
    	if(minSum != -1) {
    		return minSum;
    	}
    	minSum = 987654321;
    	
    	for(int partision = 1; start + partision <= N; partision++) {
    		minSum = Math.min(minSum, GetSum(start, start+partision-1) + Part(start+partision,count-1));
    		cache[start][count] = minSum;
    	}
    	return minSum;
    }
   
    
    public static int GetSum(int startidx, int lastidx) {
    	
    	int sum = pSum[lastidx] - (startidx == 0? 0 : pSum[startidx-1]);
    	int sqSum = pSeqSum[lastidx] - (startidx == 0? 0 : pSeqSum[startidx-1]);
    	int avg = Math.round((float)sum / (lastidx - startidx +1));
    	
    	int ret = sqSum - 2*avg*sum + avg*avg*(lastidx-startidx+1);
    	
    	return ret;
    }
}
	
````

#### 복기

처음에는 그룹을 나누는 것을, 정렬된 숫자들의 편차가 큰순으로 그룹을 나누려고 했다.

하지만 이때 문제점은, 1 1 1 4 4 4 7 7 7 7의 경우 1과 4에서도 3차이, 4와 7에서도 3차이이지만, 7이 4개이므로 (111,444)와 (7777)을 나누는게 낫다.

이런 모든 경우의 수를 고려하기에 힘들기 때문에 결국 위의 풀이에서와 같이 dp를 사용하여 모두 해보거나, dp와 조합을 사용해서 해봐야 하는 것 같다.

```java
public static void grouping() {
	
	if(S >= N) {
		ans = 0; return;
	}
	
	int index[] = new int[S+1];
	for(int i=0; i<N-1; i++) {
		distance[i] = numbers[i+1] - numbers[i];
	}
   	
	//S개를 그룹으로 나눠야함.
	for(int i=1; i<S; i++) {
		index[i] = GetIndex();
	}
	index[0] = -1;
	index[S] = N-1;

	Arrays.sort(index);

	for(int i=0; i<S; i++) {
		ans = ans + GetSum(index[i]+1, index[i+1]);
	}
}

public static int GetIndex() {
	int Max = -1;
	int index = -1;
	
	for(int i=0; i<N-1; i++) {
		
		if(distance[i] > Max) {
			Max = distance[i];
			index = i;
		}
	}
	distance[index] = -1;
	return index;
}

```


​    	



```
7
10 3
3 3 3 1 2 3 2 2 2 1
9 3
1 744 755 4 897 902 890 6 777
2 3
9 8
1 2
9
3 2
7 8 9
4 1
7 8 9 10
4 2
1000 1000 500 400
```