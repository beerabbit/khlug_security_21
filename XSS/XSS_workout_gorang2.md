# XSS workout

: 사용자 입장에서 스크립트가 실행되는 순간, 공격자가 원하는 구문이나 파일을 실행되게 만드는 방법

## #Html에서 XSS가 이용되는 대표적인 다섯 가지 태그

### 1. Script

: 가장 기본적으로 이용

### 2. a

:하이퍼링크 태그를 이용하는 방법

### 3. img

: 이미지 태그의 src를 이용하는 방법

### 4. iframe

: iframe의 src가 실행될 때 그를 이용하는 방법

### 5. link

: 역시 링크가 실행될 때를 이용.

---

## #XSS Challanges

### #4.

<img width="652" alt="Screen_Shot_2021-11-07_at_12 00 43_AM" src="https://user-images.githubusercontent.com/89091736/140616249-ff2e49e4-b076-4b26-adec-b52a7deacd07.png">

그냥 alert 스크립트를 했을 때는 <,> 등이 인코딩되어서 작동이 안 됐다.

힌트가 invisible input field라서 눈 크게 뜨고 찾아봤다 ㅋ ㅋ

24번째 줄에 아주 수상한 hackme 밸류를 가지는 태그 발견. 그래서 이 밸류를 바꾸면 바로 스크립트가 되나? 생각했다.

자바스크립트로 void(document.forms[0].p3.value='"/><script>alert(document.domain)</script>'); 을 통하여 밸류를 닫아주면

<img width="614" alt="4번" src="https://user-images.githubusercontent.com/89091736/140616245-a3c3c14c-41aa-4f17-aedb-b33b35f4fbba.png"> 

이렇게 변해서 submit했을 때 문제가 풀렸다.

### #5.

<img width="664" alt="Screen_Shot_2021-11-07_at_12 36 19_AM" src="https://user-images.githubusercontent.com/89091736/140616253-9ae38b50-59ff-4ec2-bbe3-7f79c37148ae.png">

역시 텍스트박스에 스크립트를 입력하면 인코딩이 되었다.

그래서 힌트에 따라서 텍스트박스의 길이가 제한된 부분을 주목했다.

maxlength를 강제로 늘리고 거기에 스크립트를 실행하면 되지 않을까?

void(document.forms[0].p1.maxLength="100"); 을 통해서 길이를 늘려주고

텍스트박스에

"/><script>alert(document.domain);</script>

를 입력하고 submit했더니

<img width="1078" alt="Screen_Shot_2021-11-07_at_12 40 34_AM" src="https://user-images.githubusercontent.com/89091736/140616255-1a3e7770-0e2c-4b01-80a4-565112beec1c.png">


요렇게 바뀌면서 통과되었다.
