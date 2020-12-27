## 7-2 쿼드 트리 뒤집기 (QUADTREE 난이도 : 하)

1. 그림의 모든 픽셀이 검은 색일 경우 이 그림의 쿼드 트리 압축 결과는 그림의 크기에 관계없이 b가 된다.
2. 그림의 모든 픽셀이 흰 색일 경우 이 그림의 쿼드 트리 압축 결과는 그림의 크기에 관계없이 w가 된다.
3. 모든 픽셀이 같은 색이 아니라면, 쿼드 트리는 이 그림을 가로 세로로 각각 2 등분해 4개의 조각으로 쪼갠 뒤 각각을 쿼드 트리 압축한다. 이때 전체 그림의 압축 결과는 x(왼쪽 위 부분의 압축 결과)(오른쪽 위 부분의 압축 결과)(왼쪽 아래 부분의 압축 결과)(오른쪽 아래 부분의 압축 결과)가 된다.

![쿼드트리](https://seongjaemoon.github.io/assets/uploads/algorithm/quardtree.png)

위 그림과 같이 16x16크기의 예제 그림은 쿼드 트리가 어떻게 분할해 압축하는지를 보여준다. 이때 전체 그림의 압축 결과는 xxwww bxwxw bbbww xxxww bbbww wwbb가 된다.

쿼드 트리로 압축된 흑백 그림이 주어졌을때, 이 그림을 상하로 뒤집은 그림을 쿼드 트리 압축해서 출력하는 프로그램을 작성하시오.

#### 입력

입력 입력의 첫 줄에는 테스트 케이스의 수 C(C<=50)가 주어진다. 그 후 C줄에 하나씩 쿼드 트리로 압축한 그림이 주어진다. 모든 문자열의 길이는 1,000 이하이며, 원본 그림의 크기는 220⋅220220⋅220을 넘지 않는다.

#### 출력

출력 각 테스트 케이스당 한 줄에 주어진 그림을 상하로 뒤집은 결과를 쿼드 트리 압축해서 출력한다

#### 조건

시간 및 메로리 제한 프로그램은 1초 내에 실행 되어야 하고, 64MB 이하의 메모리만 사용해야 한다.

#### 풀이

쿼드 트리 코드를 애초에 분해하지 않고, 바로바로 뒤집어 주는 방법으로 했다.

처음부터 해석하면서, w나 b가 등장하면 바로 반환해주고 재귀형식으로

코드 순서대로 왼쪽 위/ 오른쪽 위/ 왼쪽 아래/ 오른쪽 아래 를 뒤집은 형태로 반환해주었다.<img src="C:\Users\newkid\AppData\Roaming\Typora\typora-user-images\image-20200301152328231.png" alt="image-20200301152328231" style="zoom: 50%;" />



#### 코드

```java
import java.io.IOException;
import java.util.*;

public class QUADTREE {
	static StringBuilder sb = new StringBuilder();
	static int at;
	static String qt;
	static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
	
	public static void main(String[] args) throws IOException {
		int t = Integer.parseInt(br.readLine().trim());
		
		for(int i = 0; i < t; ++i) {
			pointer = 0;
			qt = br.readLine();
			if(qt.length() > 1000)continue;
			sb.append(reverse()).append("\n");
		}

		System.out.println(sb.toString());

		br.close();
	}
	
	public static String reverse() {
		//처음 글자가 x가 아니라면
		if(qt.charAt(at) != 'x') {
			at++;
            //String 형으로 리턴하기 위해 빈 문자열을 더하기.
			return qt.charAt(at- 1) + ""; 
		}else {
			//처음 글자가 x일 경우
			at++;
			//왼쪽 위.
			String ul = reverse();
			System.out.println(ul);
			//오른쪽 위.
			String ur = reverse();
			System.out.println(ur);
			//왼쪽 아래.
			String ll = reverse();
			System.out.println(ll);
			//오른쪽 아래.
			String lr = reverse();
			System.out.println(lr);

			//순서대로 뒤집기 -> 왼쪽 아래, 오른쪽 아래, 왼쪽 위, 오른쪽 위  
		return 'x' + ll + lr + ul + ur;
		}
	}
}
```



#### 복기

처음에는 이전에 쿼드트리를 풀었던 것의 반대로 생각해서, 압축된 쿼드 트리를 해독하는 함수를 짰었다.

ArrayList를 이용해서 x가 등장하면 count 를 4로 늘려줘서 x마다 4등분씩 하는 프로그램 이었는데,

쿼드 트리가 커질수록 시간과 메모리 소모량이 어마어마해서 결국 그만두었다.

분할 정복에 대해 아직도 감이 잘 안 오는 것 같다. 이렇게 간단하게 풀 수 있어서 놀라웠다.