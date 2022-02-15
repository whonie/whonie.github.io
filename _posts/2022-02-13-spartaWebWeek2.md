---
title : "[스타르타] WEB 1주차: HTML, CSS, JS기초"
categories : web
tags : [스파르타코딩클럽, 웹, css, html]
published : false
---

# jQuery란?

다른사람이 미리 작성해놓은 js코드 모음집. 먼저 import 해야함.

js와 jquery를 비교해보면

Javascript로 길고 복잡하게 써야 하는 것을

```jsx
document.getElementById("element").style.display = "none";
```

jQuery로 보다 직관적으로 쓸 수 있어요. 편리하죠? :-)

```jsx
$('#id').hide();
```



번저 해당 tag에 class처럼 id를 붙여서 조작해야함. console에서 조작할 땐

`$('#id').명령어` 이런식으로 조작함.  저 명령어엔, hide(), show(), append(''~~'), .css('display') 등 다양하게 올 수 있고,

'`'의 비밀.. : let x = `<div> ....... </div> 로 처리해놓고, console에서 명령어로 append하면 그대로 붙여넣기 가능함.



## 실습

v 내가 생각했을 때 이렇게 짜면 되겠지? 하고 쭈우우욱 짜지 말고, 하나하나 콘솔로 찍어가면서 짜라. 한번에 짜서 에러ㅗ나면 상당히 복잡해진다. 꼭.. 일부로라도.. 천천히 찍어보고!!! 하면서 하자. 꼭!!

- if 뒤에 항상 ()로 묶어주자. 보기 편함1, 에러안남2.
- 주요 문법들 직접 쳐보고 쓰면서 외우자.



# 2-7

서버-client 통신.

API란? 클라이언트와 서버를 연결해주는 창구, 약속같은 것.

GET METHOD로 통신을 하면 

```
https://movie.naver.com/movie/bi/mi/basic.nhn?code=161967

위 주소는 크게 두 부분으로 쪼개집니다. 바로 "?"가 쪼개지는 지점인데요.
"?" 기준으로 앞부분이 <서버 주소>, 뒷부분이 [영화 번호] 입니다.

* 서버 주소: https://movie.naver.com/movie/bi/mi/basic.nhn
* 영화 정보: code=161967
```



````
GET 방식으로 데이터를 전달하는 방법

?  : 여기서부터 전달할 데이터가 작성된다는 의미입니다.
& : 전달할 데이터가 더 있다는 뜻입니다.

예시) google.com/search?q=아이폰&sourceid=chrome&ie=UTF-8

         위 주소는 google.com의 search 창구에 다음 정보를 전달합니다!
         q=아이폰                        (검색어)
         sourceid=chrome        (브라우저 정보)
         ie=UTF-8                      (인코딩 정보)
````

숫자 인덱싱 하는거 말고 딕셔너리에서 키값으로 접근할 때는 항상 ''붙여주기!

css는 주로 style, js는 주로 id를 쓰는듯!

