## 9-5 길 찾기 게임 (프로그래머스 2020 KAKAO BLIND RECURITMENT)

#### 문제

**문제 링크** : https://programmers.co.kr/learn/courses/30/lessons/42893

**문제 설명**

프렌즈 대학교 조교였던 제이지는 허드렛일만 시키는 네오 학과장님의 마수에서 벗어나, 카카오에 입사하게 되었다.
평소에 관심있어하던 검색에 마침 결원이 발생하여, 검색개발팀에 편입될 수 있었고, 대망의 첫 프로젝트를 맡게 되었다.
그 프로젝트는 검색어에 가장 잘 맞는 웹페이지를 보여주기 위해 아래와 같은 규칙으로 검색어에 대한 웹페이지의 매칭점수를 계산 하는 것이었다.

- 한 웹페이지에 대해서 기본점수, 외부 링크 수, 링크점수, 그리고 매칭점수를 구할 수 있다.
- 한 웹페이지의 기본점수는 해당 웹페이지의 텍스트 중, 검색어가 등장하는 횟수이다. (대소문자 무시)
- 한 웹페이지의 외부 링크 수는 해당 웹페이지에서 다른 외부 페이지로 연결된 링크의 개수이다.
- 한 웹페이지의 링크점수는 해당 웹페이지로 링크가 걸린 다른 웹페이지의 기본점수 ÷ 외부 링크 수의 총합이다.
- 한 웹페이지의 매칭점수는 기본점수와 링크점수의 합으로 계산한다.

예를 들어, 다음과 같이 A, B, C 세 개의 웹페이지가 있고, 검색어가 hi라고 하자.

![page_rank1.png](https://grepp-programmers.s3.amazonaws.com/files/production/48a36ec7fa/243a621b-f823-4ccd-99f1-2d8d3e14050d.jpg)

이때 A 웹페이지의 매칭점수는 다음과 같이 계산할 수 있다.

- 기본 점수는 각 웹페이지에서 hi가 등장한 횟수이다.
  - A,B,C 웹페이지의 기본점수는 각각 1점, 4점, 9점이다.
- 외부 링크수는 다른 웹페이지로 링크가 걸린 개수이다.
  - A,B,C 웹페이지의 외부 링크 수는 각각 1점, 2점, 3점이다.
- A 웹페이지로 링크가 걸린 페이지는 B와 C가 있다.
  - A 웹페이지의 링크점수는 B의 링크점수 2점(4 ÷ 2)과 C의 링크점수 3점(9 ÷ 3)을 더한 5점이 된다.
- 그러므로, A 웹페이지의 매칭점수는 기본점수 1점 + 링크점수 5점 = 6점이 된다.

검색어 word와 웹페이지의 HTML 목록인 pages가 주어졌을 때, 매칭점수가 가장 높은 웹페이지의 index를 구하라. 만약 그런 웹페이지가 여러 개라면 그중 번호가 가장 작은 것을 구하라.

**제한사항**

- pages는 HTML 형식의 웹페이지가 문자열 형태로 들어있는 배열이고, 길이는 `1` 이상 `20` 이하이다.

- 한 웹페이지 문자열의 길이는 `1` 이상 `1,500` 이하이다.

- 웹페이지의 index는 pages 배열의 index와 같으며 0부터 시작한다.

- 한 웹페이지의 url은 HTML의 <head> 태그 내에 <meta> 태그의 값으로 주어진다.

  - 예를들어, 아래와 같은 meta tag가 있으면 이 웹페이지의 url은 https://careers.kakao.com/index 이다.

  - <meta property=og:url content=https://careers.kakao.com/index />

- 한 웹페이지에서 모든 외부 링크는 <a href=https://careers.kakao.com/index\>의 형태를 가진다.

  - <a> 내에 다른 attribute가 주어지는 경우는 없으며 항상 href로 연결할 사이트의 url만 포함된다.
  - 위의 경우에서 해당 웹페이지는 https://careers.kakao.com/index 로 외부링크를 가지고 있다고 볼 수 있다.

- 모든 url은 https:// 로만 시작한다.

- 검색어 word는 하나의 영어 단어로만 주어지며 알파벳 소문자와 대문자로만 이루어져 있다.

- word의 길이는 `1` 이상 `12` 이하이다.

- 검색어를 찾을 때, 대소문자 구분은 무시하고 찾는다.

  - 예를들어 검색어가 blind일 때, HTML 내에 Blind라는 단어가 있거나, BLIND라는 단어가 있으면 두 경우 모두 해당된다.

- 검색어는 단어 단위로 비교하며, 단어와 완전히 일치하는 경우에만 기본 점수에 반영한다.

  - 단어는 알파벳을 제외한 다른 모든 문자로 구분한다.
  - 예를들어 검색어가 aba 일 때, abab abababa는 단어 단위로 일치하는게 없으니, 기본 점수는 0점이 된다.
  - 만약 검색어가 aba 라면, aba@aba aba는 단어 단위로 세개가 일치하므로, 기본 점수는 3점이다.

- 결과를 돌려줄때, 동일한 매칭점수를 가진 웹페이지가 여러 개라면 그중 index 번호가 가장 작은 것를 리턴한다

  - 즉, 웹페이지가 세개이고, 각각 매칭점수가 3,1,3 이라면 제일 적은 index 번호인 0을 리턴하면 된다.



------

**입출력 예**

- word : blind

- pages :

  ```
  ["<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://a.com\"/>\n</head>  \n<body>\nBlind Lorem Blind ipsum dolor Blind test sit amet, consectetur adipiscing elit. \n<a href=\"https://b.com\"> Link to b </a>\n</body>\n</html>", "<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://b.com\"/>\n</head>  \n<body>\nSuspendisse potenti. Vivamus venenatis tellus non turpis bibendum, \n<a href=\"https://a.com\"> Link to a </a>\nblind sed congue urna varius. Suspendisse feugiat nisl ligula, quis malesuada felis hendrerit ut.\n<a href=\"https://c.com\"> Link to c </a>\n</body>\n</html>", "<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://c.com\"/>\n</head>  \n<body>\nUt condimentum urna at felis sodales rutrum. Sed dapibus cursus diam, non interdum nulla tempor nec. Phasellus rutrum enim at orci consectetu blind\n<a href=\"https://a.com\"> Link to a </a>\n</body>\n</html>"]
  ```

- pages는 다음과 같이 3개의 웹페이지에 해당하는 HTML 문자열이 순서대로 들어있다.

  ````html
  <html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
      <head>
        <meta charset="utf-8">
        <meta property="og:url" content="https://a.com"/>
      </head>  
      <body>
          Blind Lorem Blind ipsum dolor Blind test sit amet, consectetur adipiscing elit. 
          <a href="https://b.com"> Link to b </a>
      </body>
  </html>
  ````

  ````html
  <html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
      <head>
        <meta charset="utf-8">
        <meta property="og:url" content="https://b.com"/>
      </head>  
      <body>
      	Suspendisse potenti. Vivamus venenatis tellus non turpis bibendum, 
      	<a href="https://a.com"> Link to a </a>
          blind sed congue urna varius. Suspendisse feugiat nisl ligula, quis malesuada felis hendrerit ut.
      	<a href="https://c.com"> Link to c </a>
      </body>
  </html>
  ````

  ````html
  <html lang="ko" xml:lang="ko" xmlns="http://www.w3.org/1999/xhtml">
      <head>
        <meta charset="utf-8">
        <meta property="og:url" content="https://c.com"/>
      </head>  
      <body>
          Ut condimentum urna at felis sodales rutrum. Sed dapibus cursus diam, non interdum nulla tempor nec. Phasellus rutrum enim at orci consectetu blind
          <a href="https://a.com"> Link to a </a>
      </body>
  </html>
  ````





#### 풀이

문제의 알고리즘 자체는 간단하다. 각각의 페이지로 나누고, 페이지의 head와 body를 나누고, url과 기본점수를 추출하면 된다.

우선 각 페이지를 저장해놓을 클래스(Page)를 선언하고, 생성할때 head와 body를 분리한다.



주로 사용할 메소드는 indexOf(String) 함수와, subString(int, int)함수이다.

JAVA의 indexOf(String) 함수는 해당 String을 포함하는 문자열 상의 index를 반환하는 함수로, 존재하지 않으면 음수를 리턴한다.

또한 이때 반환되는 index는 포함된 문자열이 시작하는 위치이기 때문에, 만약 abcdef에서 abc를 찾는 경우 0을 반환하게 된다.

indexOf(String, int)를 통해, 해당 int의 인덱스 이후부터 찾는 것도 가능하다.



subString(int,int)는 시작index부터 끝index까지 문장을 자르는 함수이다.



이때 모든 과정에서 고려해야 할 부분이, HTML 형식에 맞지 않아 head와 body를 분리할 수 없는 케이스가 존재할 수 있다는 것이다.



우선 setUrl 메소드를 통해 페이지의 head에서 url을 분리한다. 

이때 단순히 `content=\"`로만 분리한다면 문제에서 에러가 발생할 수 있다. (테스트케이스 9)

`<meta property=\"og:url\" content=\"`전부가 들어가게 조건을 설정하여야 한다.



그리고 setOutputUrl을 메소드를 통해 페이지의 body에서 외부 url을 분리한다.

외부 url은 문제의 조건에 따라 `<a href="`로만 시작하기 때문에, 이를 활용하여 분리하면 된다.



Basicscore를 통해 기본점수를 계산하는데, 이때 body 내에서 단순 문자열 이외에도  `<a href = .../>` 와 같은 모든 body 내의 부분을 점수계산에 넣어야 한다.

또한 알파벳 이외에 모든 문자를 구분자로 사용할 수 있으므로, `[^a-zA-Z]` 의 정규식을 통해 a-z, A-Z 가 아닌(^) 모든 문자로 단어들을 split하고, 

equalsIgnoreCase 메소드를 통해 대소문자 구분없이 비교해주면 된다.



마지막으로 Linescore를 통해 외부점수를 계산하기 이전에, 링크에 따라 각 페이지를 빠르게 검색하기 위해 hashmap을 사용했다.

이후 page에 따른 외부 링크를 검색하는데, 이때 외부링크에 해당하는 페이지가 없을 수 있기 때문에 그 부분도 체크를 해줘야 한다. (테스트케이스 10)





#### 코드

````java
import java.util.ArrayList;
import java.util.HashMap;
class Page{
	double basicscore = 0;
	double linkscore = 0;
	String url;
	String head;
	String body;
	ArrayList<String> outputUrl = new ArrayList<String>();
	
	public Page(String s) {
        //to start after <head>
		this.head = s.substring(s.indexOf("<head>")+6, s.indexOf("</head>"));
		this.body = s.substring(s.indexOf("<body>")+6, s.indexOf("</body>"));
	}
}
class Solution {
	
	Page[] pageClass;
	HashMap<String, Integer> pageMap = new HashMap<String, Integer>();
	
    public int solution(String word, String[] pages) {
    	int length = pages.length;
    	pageClass = new Page[length];
    	
    	double answerScore = 0;
    	double current = 0;
    	int answer = 0;
    	
    	for(int i=0; i<length; i++) {
    		pageClass[i] = new Page(pages[i]);
    		setUrl(pageClass[i]);
    		setOutputUrl(pageClass[i]);
    		setBasicscore(pageClass[i],word);
            //mapping
    		if(pageClass[i].url != null) {
    			pageMap.put(pageClass[i].url, i);
    		}
    	}
    	
    	for(int i=0; i<length; i++) {
    		setLinkScore(pageClass[i]);
    	}
    	
    	//get answer
    	for(int i=0; i<length; i++) {
    		current = pageClass[i].basicscore + pageClass[i].linkscore;
    		if(answerScore < current) {
    			answerScore = current;
    			answer = i;
    		}
    	}
        System.out.println(answer);
        return answer;
    }
    
	//parse to pages by <head> tag
    public void setUrl(Page p) {
    	if(p.head != null) {
    		String meta = "<meta property=\"og:url\" content=\"";
    		int startidx = p.head.indexOf(meta) + meta.length();
    		int endidx = p.head.indexOf("\"",startidx);
    		
    		//parse to url
    		p.url = p.head.substring(startidx,endidx);
    	}
    }
    
    //parse to page by <a> tag
    public void setOutputUrl(Page p) {
    	
    	if(p.body != null) {
    		String body = p.body;
    		String href = "<a href=\"";
    		while(body.contains(href)) {
        		int startidx = body.indexOf(href) + href.length();
        		int endidx = body.indexOf("\"", startidx);
        		p.outputUrl.add(body.substring(startidx, endidx));
        		
        		body = body.substring(endidx);
        		
        		
        	}
    	}
    }
    
    public void setBasicscore(Page p, String word) {
    	String match = "[^a-zA-Z]";
    	if(p.body != null) {
    		String[] token = p.body.split(match);
        	for(int i=0; i<token.length; i++) {
        		if(token[i].equalsIgnoreCase(word)) {
        			p.basicscore++;
        		}
        	}
    	}
    }
    
    public void setLinkScore(Page p) {
    	for(int i=0; i<p.outputUrl.size(); i++) {
    		int idx = this.pageMap.getOrDefault(p.outputUrl.get(i), -1);
    		if(idx > -1) {
    			this.pageClass[idx].linkscore += p.basicscore / p.outputUrl.size();
    		}
    	}
    }
}
````



#### 결과

