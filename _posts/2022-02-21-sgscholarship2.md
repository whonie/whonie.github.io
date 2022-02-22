---
published : false
title : '[장학금알리미] xpath와 selenium 기초' 
---

# xpath

웹페이지가 복잡해질수록 테그의 수도 많아진다. 이 많은 테그들에 쉽게 접근할 수 있게 해주는게 xpath다.

## 문법

예컨데 `body` 가 다음과 같이 주어져있다고 해보자.

```html
<학교>
    <학년 value="1학년">
        <반 value="1반">
            <학생 value='1-1-1'>김영훈</학생>
            <학생 value='1-1-2'>이홍국</학생>
            <학생 value='1-1-3'>이지원</학생>
            <학생 value='1-1-4'>박유빈</학생>
        </반>
    </학년>
    <학년 value="2학년"/>
    <학년 value="3학년"/>
</학교>

```

물론 저런 이름의 테그는 없지만.. 이렇게 많은 테그 속에서 딱 내가 원하는것에 접근하려고 하면

`/학교/학년/반/학생[3]`

이라고 하면 이지원에 접근하는 것이다. 여기서 `[]`는 n번째 요소라는 뜻이다

또는, 무조건 `id`로만 찾으려면

`//*[@id='login']` 이런식으로 찾는다. slash가 하나면 내 디렉토리 바로 아래 디렉토리란 뜻이고, slash가 두개면 지금 위치로부터 모든 하위 엘리먼트들에 대해서 다 찾아본다. 참고로 저 `value` 등의 값은 어트리뷰트라고 한다. `*`은 테그(엘리먼트) 값에 상관없이 다 찾는다는 뜻이다.

​	

# requests

​	

```python
res = requests.get('http://www.nadocoding.tistory.com/')
```

으로 원하는 url을 가져온다.

```
res.status_code
```

는 돌아오는 코드를 출력한다. 200이면 정상. 에러처리를 if/else로 해줘도 되지만

```
res.raise_for_status()
```

를 쓰면 에러가 안나면(200이면) 쭉 진행하고, 에러가 나면 에러코드 토해내고 거기서 멈춘다.

```
print(res.text)
```

`.text`로 가져온걸 출력할 수 있다. 정상적으로 가져왔으면 html전체가 출력된다

​	

# selenium

```python
from selenium import webdriver
```

으로 먼저 불러와주고, webdriver를 다운받는다. 이 때 내가 사용하는 chrome의 버전과 맞는 webdriver를 받아야함에 유의하자.

```python
browser = webdriver.Chrome()
```

으로 시작한다.

터미널에서 조작하면 

```python
browser.get('http://naver.com')
```

이라고 하면 바로 열린다.

여기서 로그인 창을 열고싶으면 먼저 개발자도구에서 해당 엘리먼트를 살펴보자

![image-20220222230513821](../assets/images/2022-02-21-sgscholarship2/image-20220222230513821.png)

보니까 class명이 `link_login`이다. 이걸 바탕으로 하면

```python
elem = browser.find_element_by_class_name('link_login')
```

`elem`에 저 로그인 버튼이 똭!! 저장이 된다. 뭐 구문이 너무 직관적이여서 바로 이해가 간다.

그리고

```python
elem.click()
```

으로 실행하면

![image-20220222230901086](../assets/images/2022-02-21-sgscholarship2/image-20220222230901086.png)

로그인창이 뜬다!!

그 외의 문법들은 다음과 같다.

```python
browser.click()
browser.back()
browser.forward()
browser.refresh()
browser.back()
```



이번엔 주소창에 접근해보자. 이번엔 id값으로 접근한다.

```
 elem = browser.find_element_by_id('query')
```



`Keys.Enter` 를 해야하는데, 이를 위해 다음과 같은 라이브러리를 또 갖다쓴다.

```
from selenium.webdriver.common.keys import Keys
```



```
elem.send_keys('나도코딩')
```

라고 하면 입력이 된다 주소창에. 

그리고 검색하려고 엔터를 누른다.

```
 elem.send_keys(Keys.ENTER) 
```



