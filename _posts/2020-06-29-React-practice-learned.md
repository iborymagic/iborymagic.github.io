---
layout: post
title: React 실습하며 배운 내용들
tags:
  - TIL
  - react
---

### Javascript의 Date 객체로 어제, 내일 구하기
Javascript의 Date 객체를 이용하면  
하루 이전, 이후의 날짜 객체를 간단히 구할 수 있음.  

```javascript
const prevDate = new Date(${오늘 날짜 객체}.setDate(${오늘 날짜 객체}.getDate() - 1));
const nextDate = new Date(${오늘 날짜 객체}.setDate(${오늘 날짜 객체}.getDate() + 1));
```

### componentDidMount()
component가 마운트된 직후, 즉 <u>트리에 삽입된 직후에 호출된다.</u>  
화면을 처음 불러올 때 setState로 값을 설정하고 싶을 때 이용하는 듯.  

아마, <u>DOM 노드가 필요한 초기화 작업</u>을 얘한테서 해주면 될 듯 하다.  

얘는 기본적으로 render() 다음에 호출되는데,  
setState를 호출하면 render()가 한 번 더 호출되기 때문에  
render()가 두 번 발생하는 문제가 생기긴 함. 그래서 성능 문제가 발생할 수 있음.  
그래도 필요한 경우에는 쓰는 게 맞다.  

### state의 올바른 사용법
1. setState를 이용해서 고칠 것.
2. state는 비동기적으로 update 된다.
```javascript
// Wrong
this.setState({
  counter : this.state.counter + this.props.increment
});
```
state와 props는 비동기적으로 수정되기 때문에  
위와 같이 사용할 경우 우리가 원하는 state나 props를  
올바르게 가져오지 못 할 가능성이 크다.  
(아직 update가 안되었다거나, 이미 다른 값으로 바뀌었다거나).  
대신, <u>이전 상태의 state와 props를 인자로 받아올 수가 있다.</u>  
그래서, 이렇게 쓰면 된다.  
```javascript
// Correct
this.setState((state, props) => ({
  counter : state.counter + props.increment
}));
```

3. state 수정은 병합적으로 일어난다.
```javascript
this.state = {
  posts : [],
  comments : []
}
```
우리가 만약 위의 state에서 posts 배열을 고칠 경우  
comments는 전혀 손대지 않고, posts만 완벽하게 수정된다.  
분리해서 원하는 부분만 수정하고, 다시 합치는 것을 상상하면 됨.  

### Javascript 빈 값 체크
Javascript에서는 간단하게 빈 값을 체크하는 방법이 있다.  
```javascript
var value = null;
if(!value) {
  // 빈 값
} else {
  // 빈 값 아닐 때
}
```
요렇게 체크해주면 된다.  
자바스크립트 자료형에서 <u>"", null, undefined, 0, NaN</u>이  
<u>전부 false</u>로 반환된다는 점을 이용한 체크 방법.  

추가) [출처](https://sanghaklee.tistory.com/3)  
만약 들어올 수 있는 자료형 중에서  
빈 배열이나 빈 객체가 들어올 수도 있는 경우에는  
다음과 같이 명시적으로 체크를 해줘야 한다.  
```javascript
// [], {} 도 빈값으로 처리 
var isEmpty = function(value){ 
  if( value == "" || value == null || value == undefined || 
  ( value != null && typeof value == "object" && !Object.keys(value).length )) { 
    return true 
  } else { 
    return false 
  } 
};

```

### Github 원격저장소
1. github에 저장소 새로 만들기
2. github 저장소 주소 클립보드에 복사해두기
3. 프로젝트 폴더로 간다.
- `git init`
: git을 개시하는 명령어. 맨 처음에 한 번만 해주면 됨.
- `git add .`
: 변경 사항들을 stage에 올려놓는다.
- `git commit -m "커밋 메시지"`
: 변경 사항들을 커밋
- `git remote add origin "원격 저장소 주소"`
: 원격 저장소와 연동하는 명령어. 얘도 처음에 한 번만.
- `git push -u origin master` or `git push`
: (master branch에다가 혹은 그냥) push
[git을 이용한 workflow를 잘 설명해놓은 블로그](https://wordbe.tistory.com/entry/Git-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95-%EC%A0%95%EB%A6%ACcommit-push-pull-request-merge-%EB%93%B1)