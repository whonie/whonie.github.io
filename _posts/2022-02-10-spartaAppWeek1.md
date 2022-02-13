---
title : "lol"
published : false
---



1-3강

**공식문서를 주로 참조해라(mdn mozilla)**

js 기초문법 - 1

변수선언 : let. const로도 선언할 수 있다. 그러나 let과는 다르게 const로는 재할당이 불가하다. 뭐 pi같은거 쓸 때 필요할듯?

콘솔창 클리어 : 상단 clear console 버튼

아예초기화 : f5

자료형 : 숫자, 문자형(따움표)

문자열은 +로 더할 수 있다.

변수명의 약속..? : camel case vs snake case. camel : firstName, snake : first_name



js 기초문법 - 2

*list(리스트)* : let a_list = [1,2,3,4,5,'건희',6,'kim]. 문자, 숫자 모두 다 담을 수 있음

 인덱싱 : a_list[0] 하면 = 1임. 

추가 : a_list.push('sparta') 하면 맨 마지막 자료로 추가됨

리스트 길이 : a_list.length 이런 '.'을 연결연산자라고 함.



*dictionary*(딕셔너리) : let a_dict = {"name" : "younghoon", "age" : 30}. 각각 key, value라고 함

인덱싱은 key로 함. console.log(a_dict["name"]). 또는 또는 a_list.name으로 꺼낼수도있음.

추가 : a_dict["height"] = 180; v! list랑 추가법이 다름에 주의



1-4

js 기초문법 - 3

and : &&, or : ||

 for문 : for(let i=0, i<10, i++) {

​	console.log(i)

}



1-6

js 최신문법

arrow function
`let a = () => { 
 console.log("arrow function"); `
`} `
`a();.`



할당방식( 동적 분해 할당(Destructuring assignment)) 이거 정말 어렵고 신기.

```
//기존 할당 방식
let owner = blog.owner
let getPost = blog.getPost()

//비구조 할당 방식
let { owner, getPost } = blog;       
//각각 blog 객체의 owner , getPost() 의 데이터가 할당
//blog의 키 값과 이름이 같아야 해요!
//(예 - owner가 아니라 owner2를 넣어보세요! 아무것도 안 들어온답니다.)

** 앞으로 리액트 네이티브 앱을 만들며 가장 많이 사용할 방식**

//함수에서 비구조 할당 방식으로 전달된 딕셔너리 값 꺼내기
let blogFunction = ({owner,url,getPost}) => {
	console.log(owner)
	console.log(url)
	console.log(getPost())
}
blogFunction(blog)
```



마지막 함수는 파라미터 레벨에서 비구조 할당 방식을 사용하는거고, 이는 함수 내에서만 쓰이고 글로벌하게는 쓰이지 않는다!!! 반면

```
let { owner, getPost } = blog;
```

라고 하면 ` 딕셔너리의 키를 그대로 변수로 빼내서 할당한다고 보시면 될 거 같습니다!` 라고 한다..

`으로 줄바꿈을 간단하게하자!

`제 이름은 ${name(변수명)}` 이라고 써서 배열 가능!





