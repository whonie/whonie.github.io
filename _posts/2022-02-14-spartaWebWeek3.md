---
title : ''
published : false
---

일단 크롤링 어렵다.



```python
import requests
from bs4 import BeautifulSoup

# 타겟 URL을 읽어서 HTML를 받아오고,
headers = {'User-Agent' : 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36'}
data = requests.get('https://movie.naver.com/movie/sdb/rank/rmovie.nhn?sel=pnt&date=20200303',headers=headers)

# HTML을 BeautifulSoup이라는 라이브러리를 활용해 검색하기 용이한 상태로 만듦
# soup이라는 변수에 "파싱 용이해진 html"이 담긴 상태가 됨
# 이제 코딩을 통해 필요한 부분을 추출하면 된다.
wholehtml = BeautifulSoup(data.text, 'html.parser')
trs= wholehtml.select('#old_content > table > tbody > tr')

#old_content > table > tbody > tr:nth-child(2)
#old_content > table > tbody > tr:nth-child(3)
#old_content > table > tbody > tr:nth-child(4)
##old_content > table > tbody > tr:nth-child(2) > td.title > div > a
#old_content > table > tbody > tr:nth-child(2) > td:nth-child(1) > img
#old_content > table > tbody > tr:nth-child(4) > td:nth-child(1) > img
#old_content > table > tbody > tr:nth-child(4) > td:nth-child(1) > img
#old_content > table > tbody > tr:nth-child(2) > td.point

for tr in trs :
    title_tr = tr.select_one('td.title > div > a')
    rank_tr = tr.select_one('td:nth-child(1) > img')
    star_tr = tr.select_one('td.point')

    if title_tr is not None:
        title = title_tr.text
        rank = rank_tr['alt']
        star = star_tr.text
        print(rank, title, star)
```



```python
headers = {'User-Agent' : 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36'}
```

이건 '마치 직접 클릭해서 들어가는 효과'를 내는것이라고 한다. 아마 크롤러를 막는 사이트에 대처하기 위함인듯.



```python
data = requests.get('https://movie.naver.com/movie/sdb/rank/rmovie.nhn?sel=pnt&date=20200303',headers=headers)
```

이건 가져온 raw데이터 전체를 담는것. 아직 하나도 정제가 안돼있다!



```python
wholehtml = BeautifulSoup(data.text, 'html.parser')
```

이건 이제 볼 수 있는 html형태로 변환된거. `print(wholehtml)` 하면 해당 url html전체가 나온다고 보면 된다.



```python
lines= wholehtml.select('#old_content > table > tbody > tr')
```

여기서부터 (어렵고) 중요한데.. `.select()` 는 이 많은 html중에서 뭘 선택할지를 고른다. 즉 저 괄호에는 selector가 들어간다. 위의 경우엔 id가 들어갔다.

여기서 잠깐 selector란? 지금까지 크게 1. 전체선택자, 2. class 3. id 이렇게 3개를 배웠는데 복습을 하고 가자면

**전체선택자**는 폰트 적용할 때 이렇게 `<style>` 안에서 모든 테그에 css적용해줬다.

```css
* {
font-family: 'Jua', sans-serif;
}
```

이 때, 전체선택자는 테그선택자보다 우선순위가 낮음에 유의하자

**class**는 가장 많이 썼는데, css를 입힐 때 썼다.

```css
.someClass {
    color : red;
}
```

이런식이다.



**id**는 주로 js를 적용할 때 썼다. 예컨데 뭐 특정 태그 아래를 싹 다 지우거나, 특정 html코드를 append 하거나 하는 식이다.

```js
function q2() {
    let given = $('#input-q2').val();
    if (given.includes('@')) {
    let ary = given.split("@");
    let domain = ary[1];
    alert(domain);
    }
```

이 경우엔 input의 값을 .val()로 받아서 조작한다.



하여간 다시 위의 예제로 올라가면

```python
lines= wholehtml.select('#old_content > table > tbody > tr')
```





```python
```





```python
```

```python
```

```python
```

```python
```

```python
```

# mongo DB

네가지만 기억하자 insert, find, update, delete

## insert

```python
from pymongo import MongoClient
client = MongoClient('localhost', 27017) //내 로컬에 있는 mongodb로 접근한다
db = client.dbsparta //'dbsparta'라는 이름으로 접근한다

doc = {'name':'bobby','age':21} //딕셔너리 만들고
db.users.insert_one(doc) //'users'라는 컬렉션에 넣는다. 컬렉션은 비슷한애들의 집합
```



## find

```python
from pymongo import MongoClient           # pymongo를 임포트 하기(패키지 인스톨 먼저 해야겠죠?)
client = MongoClient('localhost', 27017)  # mongoDB는 27017 포트로 돌아갑니다.
db = client.dbsparta                      # 'dbsparta'라는 이름의 db를 만듭니다.

# MongoDB에서 데이터 모두 보기
all_users = list(db.users.find({}))

# 참고) MongoDB에서 특정 조건의 데이터 모두 보기. 그냥 find라고 쓰면 출력값이 여러개라서 꼭 list로 묶어줘야함.
same_ages = list(db.users.find({'age':21},{'_id':False})) // 21살인 애들을 모두 출력
same_ages는
[{'name': 'bobby', 'age': 21}
{'name': 'jane', 'age': 21}[]
임. 즉, list임. 

print(all_users[0])         # 0번째 결과값을 보기
print(all_users[0]['name']) # 0번째 결과값의 'name'을 보기

for user in all_users:      # 반복문을 돌며 모든 결과값을 보기
    print(user)
 
user = db.users.find_one({'name':'bobby'})
# find_one은 하나만 찾음. 따라서 리스트로 묶어주지 않아도 됨
# 해당 조건에 해당하는 값이 여러개면 제일 위에있는 일치하는거 하나만 출력됨
```



## update

```python
db.users.update_one({'name':'bobby'},{'$set':{'age':19}})
```

이다. 이건 코드가 길고 어려워서 거의 다 외워서 쓴다.



## 자주 사용하는 코드들 요약

```python
# 저장 - 예시
doc = {'name':'bobby','age':21}
db.users.insert_one(doc)

# 한 개 찾기 - 예시
user = db.users.find_one({'name':'bobby'})

# 여러개 찾기 - 예시 ( _id 값은 제외하고 출력)
same_ages = list(db.users.find({'age':21},{'_id':False}))

# 바꾸기 - 예시
db.users.update_one({'name':'bobby'},{'$set':{'age':19}})

# 지우기 - 예시
db.users.delete_one({'name':'bobby'})
```







```python
```

```python
```

```python
```

```python
```

```python
```

``````python
```

```python
```

```python

```

```python
```