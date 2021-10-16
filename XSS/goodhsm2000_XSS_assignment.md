## **xss에 이용되는 태그들 대표적인것 5개**

1. 가장 기본적인 script 태그
    
    <script>alert("hi")</script> 
    
    가장 기본적인 구문으로 대놓고 script태그를 넣어 alert를 실행시키는 구문이다.
    
    이 구문이 실행된다면 해당 사이트는 XSS에 대한 대책이 전혀 없다고 볼 수 있다.
    
2. a 태그 
    
    <a onmouseover="alert('hi')">
    
    a 태그를 이용한 공격이다. onmouseover 이벤트를 통해 공격을 실행한다.
    
3. img태그
    
    <img src=# onerror="alert('hi')">
    
    img 태그를 이용한 공격이다. 고의적으로 없는 src주소를 지정하고 onerror 이벤트를 통해 공격을 실행한다.
    
4. iframe태그
    
    iframe 태그를 이용하여 악성 코드가 포함된 사이트로 세션을 연결해 자동으로 프로그램을 다운로드 시킬 수 있다. 
    
    <iframe src="악성 코드가 포함된 사이트" name "Click Here" width= "0" height="0" frameborder="0"> </iframe>
    
5. body태그
    
    온로드 속성 또는 배경 속성과 같은 더 모호한 속성을 사용하려 XSS 페이로드를 <body>태그 안으로 전달할 수 있다.
    
    <body onload=alert("XSS")>
    

## XSS Challenges Stage #4

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/153544a1-6812-4052-9b78-c23aaa2bca1f/Untitled.png)

3번과 똑같은 방식으로 진행을 해봤더니 오류가 났다.

힌트를 살펴보니 보이지 않는 field가 있다고 하길래 소스코드를 살펴보았더니 위의 그림과 같이 소스코드가 히든으로 숨겨겨 있는 것을 확인할 수 있었다.  해당 type을 눈에 보이도록 text로 바꾸고

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a4f667e6-7870-4577-bc21-cb1a330d2a47/Untitled.png)

value 값으로 "><script>alert(document.domain);</script><"을 입력하여 XSS공격을 성공시켰다.

## XSS Challenges Stage #5

이 문제에서는 value에 값이 입력되는 것을 보고 "><script>alert(document.domain);</script>를 입력하려고 했지만 글자가 적히다 말았다. 소스코드를 살펴보니 maxlength를 15로 제한한 것이 원인이었다. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bb70df31-8545-4627-99fe-9a3acf09bfc7/Untitled.png)

그래서 maxlength값을 15대신 넉넉하게 70으로 잡고 "><script>alert(document.domain);</script>를 입력하여 XSS공격을 성공시켰다.
