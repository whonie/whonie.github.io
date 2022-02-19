---
title : ''
published : false
---

1. 서버는 '별개의 컴퓨터' 같은 개념이 아니라 그냥 '프로그램'이다.

2. 근데 '보통은' 컴터 하라를 '사서'(or like aws)거기 위에다가 돌림. 내가 내 컴퓨터를 24시간 돌리지 않아도 서버는 돌아갈 수 있어야 하기에.

3. flask를 사용할때는 기억해야할 네가지가 있다. 이 4가지를 일단 세팅하고 시작하자

   1. `app.py`라는 파일을 생성. 이 파일에서 서버사이드를 제어한다.

   2. templates/index.html . 일단 여저가 home으로 오면 `app.py`에서 index.html을 뱉어낸다. 따라서 index.html이 client side다. 그러면 동적변화같은건? 뭔가 클라이언트 측에서 서버로 컨택을 해야할일이 있으면(get/post) 클라이언트에서 서버로 ajax call로 요청을 보낸다. 그 요청을 받아서 서버에서 다시 정보를 뽑아낸다음에, json으로 정리하고, 이걸 json으로 묶어서 클라이언트로 보낸다. 이렇게 서버, 클라이언트 둘의 만나는 접점을 api라고 한다(은행창구 ㅋㅋ). 이 이렇게 받은 정보를 client는 뭐 jquerry를 쓰든 해서 알아서 잘 처리한다.

   3. 따라서 api를 만들고 사용할때는 서버와 클라이언트가 서로가 약속한 규칙에 따라 잘 연결되고있는지 먼저 파악해야한다. 예컨데, 특정 기능을 구현할 때 서로가 가르키는 url이 맞는지 체크한다.

   4. static

      따라서, 다음과 같은 순서로 개발한다.

      1. 클라이언트와 서버 확인하기
      2. 서버부터 만들기
      3. 클라이언트 만들기
      4. 완성 확인하기

      이게 서버a-z까지 다 끝내고 그 다음에 클라 시작하라는게 아니다. 어떤 기능을 개발할 때 서버 먼저 구현하고, 그 다음에 클라를 구현하라는 말이다.

# get과 post 복습

**get**은 url통해서 정보를 전달한다. 즉 client가 server로 전달하는 정보가 가시적으로 보인다. ?뒤가 보내느정보고, &를 사용해서 여러개의 정보를 보낸다.

**post**는 별개의 딕셔너리를 동봉? 해서 정보를 보낸다.



# '모두의책리뷰' 만들기

## 기본코드분석

현재 나에게 주어진(졌다고 가정하는) 파일들을 분석해보자. 그래야 추가해야할 기능도 알 수 있으니까.

먼저 서버사이드의 `app.py` 를 각주를 달아가며 분석해보자

```python
from flask import Flask, render_template, jsonify, request #여기까진 기본 약간 템플릿 느낌..
app = Flask(__name__) #여기까진 기본 약간 템플릿 느낌..

from pymongo import MongoClient #mongoDb위한 코드
client = MongoClient('localhost', 27017) #로컬서버의 27017로 접근하겠다.
db = client.dbsparta #이제 db.xxx로 db를 제어할 수 있다.


@app.route('/') #웹페이지 딱 접속하자마자 뜨는부분. shop.sparta.com이면 뒤에 slash가 생략돼있다고 봐도 됨.
def home():
    return render_template('index.html')

## API 역할을 하는 부분
@app.route('/review', methods=['POST']) # /review라는 url로 post요청이 왔을 때 서버에서 처리하는 부분
def write_review():
    sample_receive = request.form['sample_give']
    print(sample_receive)
    return jsonify({'msg': '이 요청은 POST!'})


@app.route('/review', methods=['GET']) #review라는 url로 get요청에 왔을 떄 서버에서 처리하는 부분
def read_reviews():
    sample_receive = request.args.get('sample_give')
    print(sample_receive)
    return jsonify({'msg': '이 요청은 GET!'})


if __name__ == '__main__':
    app.run('0.0.0.0', port=5000, debug=True)
```

​	

다음으로 `index.html` 을 알아보자. 아까도 말했지만 `index.html` 은 templates 안에있고, flask서버에 '/'도메인의 요청을 받으면 `index.html` 을 클라이언트로 쏴줘서 그려진다.

 ```html
 <!DOCTYPE html>
 <html lang="ko">
 
     <head>
         <!-- Webpage Title -->
         <title>모두의 책리뷰 | 스파르타코딩클럽</title>
 
         <!-- Required meta tags -->
         <meta charset="utf-8">
         <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
 
         <!-- Bootstrap CSS -->
         <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css"
               integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm"
               crossorigin="anonymous">
 
         <!-- JS -->
         <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
         <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js"
                 integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q"
                 crossorigin="anonymous"></script>
 
         <!-- 구글폰트 -->
         <link href="https://fonts.googleapis.com/css?family=Do+Hyeon&display=swap" rel="stylesheet">
 
         <script type="text/javascript">
 
             $(document).ready(function () {
                 showReview();
             });
 
             function makeReview() {
                 $.ajax({
                     type: "POST",
                     url: "/review",
                     data: {sample_give:'샘플데이터'},
                     success: function (response) {
                         alert(response["msg"]);
                         window.location.reload();
                     }
                 })
             }
 
             function showReview() {
                 $.ajax({
                     type: "GET",
                     url: "/review?sample_give=샘플데이터",
                     data: {},
                     success: function (response) {
                         alert(response["msg"]);
                     }
                 })
             }
         </script>
 
         <style type="text/css">
             * {
                 font-family: "Do Hyeon", sans-serif;
             }
 
             h1,
             h5 {
                 display: inline;
             }
 
             .info {
                 margin-top: 20px;
                 margin-bottom: 20px;
             }
 
             .review {
                 text-align: center;
             }
 
             .reviews {
                 margin-top: 100px;
             }
         </style>
     </head>
 
     <body>
         <div class="container">
             <img src="https://previews.123rf.com/images/maxxyustas/maxxyustas1511/maxxyustas151100002/47858355-education-concept-books-and-textbooks-on-the-bookshelf-3d.jpg"
                  class="img-fluid" alt="Responsive image">
             <div class="info">
                 <h1>읽은 책에 대해 말씀해주세요.</h1>
                 <p>다른 사람을 위해 리뷰를 남겨주세요! 다 같이 좋은 책을 읽는다면 다 함께 행복해질 수 있지 않을까요?</p>
                 <div class="input-group mb-3">
                     <div class="input-group-prepend">
                         <span class="input-group-text">제목</span>
                     </div>
                     <input type="text" class="form-control" id="title">
                 </div>
                 <div class="input-group mb-3">
                     <div class="input-group-prepend">
                         <span class="input-group-text">저자</span>
                     </div>
                     <input type="text" class="form-control" id="author">
                 </div>
                 <div class="input-group mb-3">
                     <div class="input-group-prepend">
                         <span class="input-group-text">리뷰</span>
                     </div>
                     <textarea class="form-control" id="bookReview"
                               cols="30"
                               rows="5" placeholder="140자까지 입력할 수 있습니다."></textarea>
                 </div>
                 <div class="review">
                     <button onclick="makeReview()" type="button" class="btn btn-primary">리뷰 작성하기</button>
                 </div>
             </div>
             <div class="reviews">
                 <table class="table">
                     <thead>
                     <tr>
                         <th scope="col">제목</th>
                         <th scope="col">저자</th>
                         <th scope="col">리뷰</th>
                     </tr>
                     </thead>
                     <tbody id="reviews-box">
                     </tr>
                     </tbody>
                 </table>
             </div>
         </div>
     </body>
 
 </html>
 ```

두개 중심으로 살펴보자.

1. 서버랑 어디에서 통신하는지.(ajax)
2. 동적인 부분이 있는지(jquerry)



먼저 서버랑은 ajax를 call해서 통신할텐데 showReview()와 makeReview()를 통해 통신한다. 서버가 있는 이상 index.html은 껍데기일 뿐임으로.

showReview()는 모든 로딩이 다 끝난 뒤 호출된다. 즉, 서버에서 리뷰정보를 받아와서 display해주면 될것이다.

makeReview()는 버튼이 눌릴 때 호출당하는데, input데이터를 받아서 서버로 보내준다. 

그럼 이제 구현해야하는건 다음과 같다.

1. 서버사이드 :
   1. 들어온 get 요청에 따라 서버에 저장된 내용을 보내준다.
   2. 들어온 포스트 요청에 따라 서버에 저장한다 들어온 데이터
2. 클라이언트 사이드:
   1. 다 로딩이 끝나면 서버로부터 리뷰데이터를 받아서 뿌려준다.
   2. 버튼을 누르면 리뷰를 보낸다.



먼저 클라이언트 측으로 저장된 데이터를 뿌려준다. 그럴려면 db에 있는 데이터를 가져와야한다.

---

2018

즉, 설계 = 우리 프로젝트에 어떤 어떤 기능이 필요하고 이를 어떻게 구현할것인지 짜는것.

위의 `html`의 구조는 다음과 같다.

일단 db의 내용이 들어가는 부분을 제외한 모든 부분이 먼저 로딩된다. 즉, `body` 에서 `<tbody id="reviews-box">` 위의 부분이다. 얘네들이 다 로딩되면

```javascript
$(document).ready(function () {
	showReview();
});
```

이걸로 인해서 `showreview()`가 호출되고 db에 있는 정보를 불러와서 review id에 append된다.

---

## callback function()

callback함수 정의: 함수에 파라미터로 들어가는 함수. 용도 :  순차적으로 실행하고 싶을 때 씀. 예컨데

```javascript
document.querySelector('.button').addEventListener('click',function(){
    
})
```

여기서는 button을 *클릭했을 때* 대괄호 안의 구문이 실행된다. addEventListener는 함수고, *파라미터로 fucntion이* 들어가있다. 

```javascript
setTimeout(function(){
    
}, 1000)
```

여기서도 보면  setTimeout func의 *파라미터로 function()*이 들어가고, 이 function()의 내용인 대괄호 안의 내용이 *1000ms 뒤에* 실행되는것을 볼 수 있다

유튜브 코딩애플 참조

---

## Ajax

결국 ajax는 js로 서버와 통신하는 방법이고, 이ajax를 쓰는 방법이 여러개가 있는데 그중에서 한게가 jQuerry에 탑재돼있는 기능을 활용하는거다. client에서 보낼때는 data에 딕셔너리 형식으로 넣는다. 그러면 flask로 받을 때 따로 dict(~~)안해도 바로 일단 dict처럼 사용 가능하다. jQuerry에서 ajax로 보낼 때 dict안에 value로서 list를 넣는다거나 그러면 잘 안들어간다. 일단 정확한 원인과 해결방안을 찾기 전까지는 무조건 일반적인 dict형태로 자료를 넘기자.

Ajax의 비동기식/동기식 처리 :

먼저 동기식과 비동기식의 차이를 알아보자. 동기식은 함수를 호출하고, return이 올때까지 기다려준다. 그러니까 함수 실행이 끝나기 전에 다음 구문을 실행하지 않는다. 예컨데

```javascript
function first() {
    ...
    return firstOutput
}

function second() {
    ...
    return secondOutput
}

let a = first();
let b = second();
```

인데 이게 동기식 처리면 first가 다 끝나고 값을 return 받은 다음에 다음 구문으로 넘어가고, 비동기식이면 return이 왔든 안왔든 second()로 바로 넘어간다. 하지만 비동기식 프로그램에선 그래서 값이 왔는지 안왔는지 알 수 있어야 하기때문에 callback함수란게 있다. 

```javascript
        function appendList() {
            $.ajax({
                type: "GET",
                url: "/order",
                data: {},
                async: false,
                success: function (response) {
                    let receive = response
                    for (let i = 0; i < receive.length; i++) {
                        let name = receive[i]['name']
                        let howmany = receive[i]['howmany']
                        let addr = receive[i]['addr']
                        let phone = receive[i]['phone']
                        let rank = i+1
                        tempHtml = `<tr>
                                        <th scope="row">${rank}</th>
                                        <td>${name}</td>
                                        <td>${addr}</td>
                                        <td>${howmany}</td>
                                        <td>${phone}</td>
                                    </tr>`
                        $('#orderList').append(tempHtml)
                    }
                    // console.log(receive)
                }
            })
        }
```

~~예컨데 여기서 보면 지금은 `async:false`여서 동기식 처리가 되고있다. 만약 비동기식 처리였다면 값이 나온 다음에 `success`콜백함수가 실행됐을것이다. 만약 async:false을 지워서 비동기식처리를 하고 아래에 각주처리된 `console.log`를 정상적으로 실행시킨다면 receive의 값은  undefine이 될것이다. 왜? receive가 들어올때까지 기다리지 않고 바로 console.log() 가 돌아가서 그렇다.~~











