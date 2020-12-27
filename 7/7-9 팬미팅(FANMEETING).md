## 	7-9 팬미팅(FANMEETING)



#### 문제 정보

ID : FANMEETING  / 시간제한 : 15000ms / 메모리제한 65536KB

#### 문제

가장 멤버가 많은 아이돌 그룹으로 기네스 북에 올라 있는 혼성 팝 그룹 하이퍼시니어가 데뷔 10주년 기념 팬 미팅을 개최했습니다. 팬 미팅의 한 순서로, 멤버들과 참가한 팬들이 포옹을 하는 행사를 갖기로 했습니다. 하이퍼시니어의 멤버들은 우선 무대에 일렬로 섭니다. 팬 미팅에 참가한 M명의 팬들은 줄을 서서 맨 오른쪽 멤버에서부터 시작해 한 명씩 왼쪽으로 움직이며 멤버들과 하나씩 포옹을 합니다. 모든 팬들은 동시에 한 명씩 움직입니다. 아래 그림은 행사 과정의 일부를 보여줍니다. a~d는 네 명의 하이퍼시니어 멤버들이고, 0~5는 여섯 명의 팬들입니다.

![img](http://algospot.com/media/judge-attachments/bcbb33d48baf27a4347cf09608aece35/Screenshot%20from%202013-01-14%2022%3A31%3A18.png)

하지만 하이퍼시니어의 남성 멤버들이 남성 팬과 포옹하기가 민망하다고 여겨서, 남성 팬과는 포옹 대신 악수를 하기로 했습니다. 줄을 선 멤버들과 팬들의 성별이 각각 주어질 때 팬 미팅이 진행되는 과정에서 하이퍼시니어의 모든 멤버가 동시에 포옹을 하는 일이 몇 번이나 있는지 계산하는 프로그램을 작성하세요.

#### 입력

첫 줄에 테스트 케이스의 개수 C (C≤20)가 주어집니다. 각 테스트 케이스는 멤버들의 성별과 팬들의 성별을 각각 나타내는 두 줄의 문자열로 구성되어 있습니다. 각 문자열은 왼쪽부터 오른쪽 순서대로 각 사람들의 성별을 나타냅니다.

M은 해당하는 사람이 남자, F는 해당하는 사람이 여자임을 나타냅니다. 멤버의 수와 팬의 수는 모두 1 이상 200,000 이하의 정수이며, 멤버의 수는 항상 팬의 수 이하입니다.

#### 출력

각 테스트 케이스마다 한 줄에 모든 멤버들이 포옹을 하는 일이 몇 번이나 있는지 출력합니다.



#### 풀이

처음엔 입력받은 팬,가수를 F -> 0, M->1로 치환해 이진수 숫자로 만든 뒤 and연산을 통해 0이되면 포옹을 했다고 하려 했다.

하지만 가수,팬이 10만명일 경우엔 이진수가 10만자리가 되어버리므로, 이걸 담을 형태의 표현방식이 없어서

30개씩 끊어서 계산했으나 실패. 마찬가지로 한자리씩 비교해보아도 실패했다. 

결국 카라추바 알고리즘을 사용해야 함을 느꼈는데,, 이에 관해서는  [여기](#카라추바-알고리즘(karatsuba method))

``````JAVA
import java.io.IOException;
import java.util.ArrayList;

public class Main {
	public static StringBuilder sb = new StringBuilder();
	public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
	static ArrayList<Integer> member = new ArrayList<Integer>();
	static ArrayList<Integer> fans = new ArrayList<Integer>();
	static int ans;
	
	public static void main(String[] args) throws IOException {
		
		int C = Integer.parseInt(br.readLine());
		
		for(int i=0; i<C; i++) {
			ans = 0;	
			String s = br.readLine().trim();			
			input(s,member);
			s = br.readLine().trim();
			input(s,fans);
			check(member,fans);

			
			sb.append(ans).append("\n");
			member.clear();
			fans.clear();
		}
		System.out.println(sb.toString());
		return;
	}
    
	public static void check(ArrayList<Integer> member, ArrayList<Integer> fans) {
		int chance = fans.size() - member.size() + 1;
		while((fans.size() - member.size() + 1) > 0) {
			for(int i=0; i < member.size(); i++) {
				if((member.get(i) & fans.get(i)) != 0){
					chance--;
					break;
				}
			}
			fans.remove(0);
		}
		ans = chance;
	}

	public static void input(String s, ArrayList<Integer> array) {
		for(int i=0; i<s.length(); i++) {
			if(s.charAt(i) == 'F') {
				array.add(0);
			}
			else {
				array.add(1);
			}
		}
	}
	
}
``````

```java
import java.io.IOException;

public class Main {
	public static StringBuilder sb = new StringBuilder();
	public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
	static int ans;
	
	public static void main(String[] args) throws IOException {

		int C = Integer.parseInt(br.readLine());
		
		for(int i=0; i<C; i++) {
			ans = 0;
			String mem = br.readLine().trim();
			String fan = br.readLine().trim();

			check1(mem,fan);
			
			sb.append(ans).append("\n");
		}
		System.out.println(sb.toString());
		return;
	}
	
	public static void check1(String mem, String fan) {
		int chance = fan.length() - mem.length() +1;
		
		mem = mem.replace('F','0');
		mem = mem.replace('M','1');
		fan = fan.replace('F','0');
		fan = fan.replace('M','1');
		
		while(mem.length() <= fan.length()) {
			
			String meetFan = fan.substring(0, mem.length());
			String members = mem.toString();
			int checkmeeting = 0;

			while(members.length() > 30) {			
				int checkmem = Integer.parseInt(members.substring(0,30),2);
				int checkfans = Integer.parseInt(meetFan.substring(0,30),2);
				if((checkmem & checkfans) != 0) {
					checkmeeting = 1;
				}
				members = members.substring(30);
				meetFan = meetFan.substring(30);
			}
			int member = Integer.parseInt(members,2);
			int fans = Integer.parseInt(meetFan,2);			
			int meeting = (member & fans);

			
			if(meeting != 0 || checkmeeting == 1) {
				chance--;
			}
			
			//fan 앞에 하나 날려주기
			fan = fan.substring(1);
		}
		ans = chance;
	}
}
```



## 카라추바 알고리즘(karatsuba method)

위 문제를 곱셈으로 생각해볼때, 각 자리수별 연산(C6~C0)을 생각해보면 팬과 가수가 만나는 것과 유사함을 알 수 있다.

이때 문제에서는 팬이 왼쪽에서부터 들어왔으므로, 팬을 거꾸로 뒤집어서 곱해주면 우리가 생각하는 결과를 알 수 있다.

![img](https://t1.daumcdn.net/cfile/tistory/995F1E355B1DF96C0D)

여기서 발생하는 문제점은, 10만자리 곱하기 10만자리를 일반적으로 하면 $O(N^2)$ 의 시간복잡도를 가지는데, 이러면 시간안에 할 수 없다.

그래서 카라츠바 형님이 만드신 알고리즘으로 $O(n^{\log{3}} )$ 으로 해야 한다.

카라츠바 알고리즘은 곱하는 256자리의 두 정수 a,b를 다음과 같이 나눈다.

![img](https://t1.daumcdn.net/cfile/tistory/993512365BF0DF3C2B)

![img](https://t1.daumcdn.net/cfile/tistory/992E41365BF0DF3D2B)



a1,b1은 각각 a,b의 첫 128자리, a0,b0는 각각 a,b의 뒷 128자리를 나타낸다.

이제 a \* b의 계산 과정은 다음과 같이 나눌 수 있다.

![img](https://t1.daumcdn.net/cfile/tistory/992529365BF0E0E332)

이 상태에서는 n/2 크기의 두 정수의 곱셈이 총 4번 사용된다. 이 곱셈 횟수를 줄이기 위해 다음 수식을 이용한다.



![img](https://t1.daumcdn.net/cfile/tistory/997B6A345BF0E27B38)

이 수식을 수정하면 다음의 결과를 얻을 수 있다.

$z2 = a1 \times b1 $ 				$z0 = a0 \times b0 $				$z1 = (a0+a1) \times (b0+b1) -z0 -z2) $

이렇게 수정하고 나면 $a \times b$ 는 n/2 크기의 두 정수의 곱셈 3번, 덧셈 2번, 뺄셈 2번으로 수행 할 수 있다.



그렇다면 카라츠바 알고리즘을 구현해보자

#### 카라츠바 알고리즘

```java
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class study {
	public static StringBuilder sb = new StringBuilder();
	public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
	static ArrayList<Integer> aaa = new ArrayList<Integer>();
	static ArrayList<Integer> bbb= new ArrayList<Integer>();
	static ArrayList<Integer> ans= new ArrayList<Integer>();
	
	public static void main(String[] args) throws IOException {
		String s1 = br.readLine().trim();
		String s2 = br.readLine().trim();
		
		for(int i = 0; i<s1.length(); i++) {
			aaa.add(s1.charAt(i)-'0');			
		}
		Collections.reverse(aaa);
		
		
		for(int i = 0; i<s2.length(); i++) {
			bbb.add(Integer.parseInt(s2.substring(i, i+1)));		
		}
		Collections.reverse(bbb);
		ans = karatsuba(aaa, bbb);
		
		//앞자리수 0 없애주기
		while (ans.size() > 1 && ans.get(ans.size() - 1) == 0)
			ans.remove(ans.size() - 1);
		String answer = "";	
		
		for(int i = ans.size()-1; i>=0; i--) {
			answer = answer.concat(ans.get(i).toString());
		}
		System.out.println(answer);
		
	}

	static ArrayList<Integer> karatsuba(ArrayList<Integer> a, ArrayList<Integer> b) {
		int a_size = a.size();
		int b_size = b.size();
		//A>B여야 한다
		if (a_size < b_size)
			return karatsuba(b, a);
        //만약에 비어있는 애 오면 끝
		if (a_size == 0 || b_size == 0)
			return null;
        //50이하에서는 보통 기본 곱셈이 빠르다
		if (a_size <= 50)
			return multiply(a, b);

		int half = a_size / 2;
		//a>b일때 b0 = null이 될 때도 있다. karatsuba에서나, sub, sum할때 예외처리를 해주자.
        ArrayList<Integer> a0 = new ArrayList<Integer>(a.subList(0, half));
		ArrayList<Integer> a1 = new ArrayList<Integer>(a.subList(half, a.size()));
		ArrayList<Integer> b0 = new ArrayList<Integer>(b.subList(0, Math.min(b.size(), half)));
		ArrayList<Integer> b1 = new ArrayList<Integer>(b.subList(Math.min(b.size(), half), b.size()));

		// z2 = a1 * b1
		ArrayList<Integer> z2 = karatsuba(a1, b1);
		// z0 = a0 * b0
		ArrayList<Integer> z0 = karatsuba(a0, b0);
		// a0 = a0 + a1; b0 = b0 + b1
		a0 = karatsubasum(a0, a1, 0);
		b0 = karatsubasum(b0, b1, 0);

		// z1 = (a0 * b0) - z0 - z2;
		ArrayList<Integer> z1 = karatsuba(a0, b0);
		z1 = karatsubasub(z1, z0);
		z1 = karatsubasub(z1, z2);

		// ret = z0 + z1 * 10^half + z2 * 10^(half*2)
		ArrayList<Integer> ret = new ArrayList<Integer>();
		ret = karatsubasum(ret, z0, 0);
		ret = karatsubasum(ret, z1, half);
		ret = karatsubasum(ret, z2, half * 2);

		return ret;
	}

	public static ArrayList<Integer> ensureSize(ArrayList<Integer> list, int size) {
		list.ensureCapacity(size);
		while (list.size() < size) {
			list.add(0);
		}

		return list;
	}
	
	static ArrayList<Integer> multiply(List<Integer> a, List<Integer> b){
		ArrayList<Integer> c = new ArrayList<Integer>();
		c = ensureSize(c, a.size()+b.size()+1);
		for(int i =0; i<a.size(); i++) {
			for(int j =0; j<b.size(); j++) {			
				c.set(i+j, c.get(i+j) + a.get(i)*b.get(j));
			}
		}
		c = normalize(c);
		return c;
	}
	
	//a = a + b*(10^k);
	public static ArrayList<Integer> karatsubasum(ArrayList<Integer> a, ArrayList<Integer> b, int k){
		if(b == null) {
			return a;
		}
		a = ensureSize(a, Math.max(a.size(), b.size() + k));
		for (int i = 0; i < b.size(); i++) {
			a.set(i + k, a.get(i + k) + b.get(i));
		}	
		a = normalize(a);
		return a;
	}
	
	//a= a-b ; a>=b 일때
	public static ArrayList<Integer> karatsubasub(ArrayList<Integer> a, ArrayList<Integer> b){
		if(b == null) {
			return a;
		}
		a = ensureSize(a, Math.max(a.size(), b.size()) + 1);
		for (int i = 0; i < b.size(); i++) {
			a.set(i, a.get(i) - b.get(i));
		}
		a = normalize(a);
		return a;
	}

	public static ArrayList<Integer> normalize(ArrayList<Integer> num) {
		num.add(0);
		for (int i = 0; i < num.size() - 1; i++) {
			if (num.get(i) < 0) {
				int borrow = (Math.abs(num.get(i)) + 9) / 10;
				num.set(i + 1, num.get(i + 1) - borrow);
				num.set(i, num.get(i) + borrow * 10);
			} else {
				num.set(i + 1, num.get(i + 1) + num.get(i) / 10);
				num.set(i, num.get(i) % 10);
			}
		}
		if (num.get(num.size() - 1) == 0)
			num.remove(num.size() - 1);
		return num;
	}
}
```

이를 이용해 코드를 만들어 보았다.

#### 코드

```java
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class Main {
	public static StringBuilder sb = new StringBuilder();
	public static java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(System.in));
	static ArrayList<Integer> members = new ArrayList<Integer>();
	static ArrayList<Integer> fans = new ArrayList<Integer>();
	static int ans;
	
	public static void main(String[] args) throws IOException {
		
		int C = Integer.parseInt(br.readLine());
		
		for(int i=0; i<C; i++) {
			String s1 = br.readLine().trim();
			String s2 = br.readLine().trim();	
			check(s1, s2);
			sb.append(ans).append("\n");
			members.clear();
			fans.clear();
		}
		System.out.println(sb.toString());
		return;
	}
	
	public static void check(String member, String fan) {
		for(int i = 0; i<member.length(); i++) {
			members.add(input(member.charAt(i)));
		}
		
		for(int i = 0 ; i <fan.length() ; i++) {
			fans.add(0,input(fan.charAt(i)));
		}
		ArrayList<Integer> hug = karatsuba(members, fans);
		ans = 0;
		
		for(int i = member.length() -1; i< fan.length(); i++) {
			if(hug.get(i) == 0 ) {
				ans++;
			}	
		}
	}
	
	public static int input(char s) {
			if(s == 'F') {
				return 0;
			}
			else {
				return 1;
			}
	}

	static ArrayList<Integer> karatsuba(ArrayList<Integer> a, ArrayList<Integer> b) {
		int a_size = a.size();
		int b_size = b.size();

		if (a_size < b_size)
			return karatsuba(b, a);
		if (a_size == 0 || b_size == 0)
			return null;
		if (a_size <= 50)
			return multiply(a, b);

		int half = a_size / 2;
		ArrayList<Integer> a0 = new ArrayList<Integer>(a.subList(0, half));
		ArrayList<Integer> a1 = new ArrayList<Integer>(a.subList(half, a.size()));		
		ArrayList<Integer> b0 = new ArrayList<Integer>(b.subList(0, Math.min(b.size(), half)));
		ArrayList<Integer> b1 = new ArrayList<Integer>(b.subList(Math.min(b.size(), half), b.size()));

		// z2 = a1 * b1
		ArrayList<Integer> z2 = karatsuba(a1, b1);
		// z0 = a0 * b0
		ArrayList<Integer> z0 = karatsuba(a0, b0);
		// a0 = a0 + a1; b0 = b0 + b1
		a0 = karatsubasum(a0, a1, 0);
		b0 = karatsubasum(b0, b1, 0);

		// z1 = (a0 * b0) - z0 - z2;
		ArrayList<Integer> z1 = karatsuba(a0, b0);
		z1 = karatsubasub(z1, z0);
		z1 = karatsubasub(z1, z2);

		// ret = z0 + z1 * 10^half + z2 * 10^(half*2)
		ArrayList<Integer> ret = new ArrayList<Integer>();
		ret = karatsubasum(ret, z0, 0);
		ret = karatsubasum(ret, z1, half);
		ret = karatsubasum(ret, z2, half * 2);

		return ret;
	}

	public static ArrayList<Integer> ensureSize(ArrayList<Integer> list, int size) {
		list.ensureCapacity(size);
		while (list.size() < size) {
			list.add(0);
		}

		return list;
	}

	
	static ArrayList<Integer> multiply(List<Integer> a, List<Integer> b){
		ArrayList<Integer> c = new ArrayList<Integer>();
		c = ensureSize(c, a.size()+b.size()+1);
		for(int i =0; i<a.size(); i++) {
			for(int j =0; j<b.size(); j++) {			
				c.set(i+j, c.get(i+j) + a.get(i)*b.get(j));
			}
		}
		return c;
	}
	
	//a = a*(10^k) + b;
	public static ArrayList<Integer> karatsubasum(ArrayList<Integer> a, ArrayList<Integer> b, int k){
		if(b == null) {
			return a;
		}		
		a = ensureSize(a, Math.max(a.size(), b.size() + k));
		for (int i = 0; i < b.size(); i++) {
			a.set(i + k, a.get(i + k) + b.get(i));
		}
		return a;
	}
	
	//a= a-b ; a>=b 일때
	public static ArrayList<Integer> karatsubasub(ArrayList<Integer> a, ArrayList<Integer> b){
		if(b == null) {
			return a;
		}
		a = ensureSize(a, Math.max(a.size(), b.size()) + 1);
		for (int i = 0; i < b.size(); i++) {
			a.set(i, a.get(i) - b.get(i));
		}
		return a;
	}
}
```

하지만 개 빡치는건 이렇게 만들어도 시간초과가 떴다는 것이다. 

참고로 위에 곱하기를 구현한거에서는 1000자리 *1000자리는 180ms가 걸렸다.



#### 출처

[서기리보이의 블로그 :: 카라츠바의 빠른 곱셈 (KARATSUBA ALGORITHM)](https://invincibletyphoon.tistory.com/13)

 [비락 식혜를 낳는 블로그 :: [알고스팟] 팬 미팅](https://sangdo913.tistory.com/21)

