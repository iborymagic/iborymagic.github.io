---
layout: post
title: Web Components-2
tags:
  - VanillaJS
  - TIL
  - Web Components
---
### Custom Element의 몇 가지 규칙
1. 사용자 설정 요소의 이름에는 대시(-)가 포함되어야 한다.
<x-tags>, <my-element>, <my-awesome-app> 등은 모두 유효한 이름이지만
<tabs>, <foo_bar>는 유효하지 않은 이름이다.
일반 HTML 태그 요소와 사용자 설정 요소를 구분하기 위함.
2. 동일한 태그를 두 번 이상 등록할 수 없다. DOMException이 발생함.
3. 항상 닫는 태그를 작성해줄 것(<app-drawer></app-drawer>)

### 요소 확장
Custom Elements API는 새로운 HTML 요소를 생성하는 데 유용하다.  
하지만, 다른 사용자 설정 요소를 확장하거나 기본 HTML 태그를 확장할 수도 있다.  
```javascript
// 다른 사용자 설정 요소 확장
class FancyDrawer extends AppDrawer {
    constructor() {
        super(); // 항상 호출해줄 것. 
    ....
}
customElements.define('fancy-app-drawer', FancyDrawer);

// 기본 HTML 요소 확장
class FancyButton extends HTMLButtonElement {
    constructor() {
        super();
    ...
}
customElements.define('fancy-button', FancyButton, {extends : 'button'});
// 세 번째 인자를 통해, 확장하는 기본 태그가 무엇인지를 알려야 함.
```
위에서 버튼 태그를 확장한 fancy-button 태그에는 기존의 버튼 태그에 있던  
click() 메서드와 같은 각종 DOM 속성/메서드들이 전부 상속된다.  

### 속성(Attribute)
사용자 설정 요소에는 속성을 추가할 수 있다고 했었다.  
정확하진 않지만, 아마 속성마다 getter/setter 메소드를 만들어야 해당 클래스 내부에서  
그 속성에 접근할 수 있는 권한이 생기는 것 같다.  
(getter/setter 메소드를 주석처리 하고 나니,   
해당 속성을 변경하거나 읽는 모든 작업이 돌아가지 않았다.)  

그리고 `static get observedAttributes()` 라는 메소드가 있다.  
브라우저는 observedAttributes가 return하는 배열에 나열된 속성들 중 하나가 변경될 때마다  
attributeChangedCallback()이라는 메소드를 호출한다.  

그렇기 때문에, 속성이 변경되었을 때 뭔가 작업을 해주고 싶다면,  
그 작업을 attributeChangedCallback() 메소드에다가 정의해 준 다음에 해당 속성을  
observedAttributes() 메소드가 return하는 배열에 넣어주면 된다.  

참고로, 해당 element의 모든 instance들이 속성에 관한 설정을 공유해야하기 때문에  
(속성에 관해 정의하면 그 내용이 모든 instance들에게 적용이 되어야 하기 때문에)  
observedAttributes() 메소드는 static 메소드로 선언해줘야한다고 함.  


대충 이정도까지이고, 이전에 배웠던 내용처럼  
element 내부에 shadow DOM을 만들어주고, 거기에 template 태그를 추가해서  
사용자 설정 태그를 사용하는 것이 국룰인듯.  
거의 이 세 개(Custom Elements, shadow DOM, Template)가 하나로 움직이는 듯 하다.  

조금 더 알고싶다면 Google Developer 페이지에 들어가서 공부하면 될 듯.  

좀 더 자세한 내용을 다루고 나니  
언뜻 봐도, 복잡한 component를 만들기에는 지나치게 소모적인 작업이 필요할 것 같다.  
property(태그 속성)를 만들 때마다 getter/setter 메소드도 일일이 만들어줘야 하고..  

그래서 React나 Vue와 같은 프레임워크들이 각광을 받고있는 것이 아닐까.  
별도로 사용하고싶지는 않은 기능이지만, React나 Vue와 같은 프레임워크들이  
그 뒷면에서는 어떤 식으로 동작을 하고 있는지를 이해하기에 좋은 공부가 아니었나 싶다.  

+) bind(this)는 정말 자주 쓰이는 패턴.  
자바스크립트에서, 콜백함수는 클래스 내부의 메소드에서 사용되더라도  
클래스 내부에 포함되어 있지 않은 것처럼 처리가 되는 것 같다.  
그래서, 클래스 메소드의 콜백 함수에서 this를 사용해줘도 해당 클래스가 아닌 window 객체를 가리키게 된다.  
콜백 함수는 선언되는 위치에 바인딩되는 게 아닌가보다(window로 바인딩된다).   

여튼, 그래서 콜백함수 내부에서 this가 해당 클래스를 가리키게 하려면  
콜백함수의 정의가 끝나고, 밖에서 bind(this)를 통해 클래스를 this로 바인딩해줘야 한다.  

[a shot of code 유튜브](https://www.youtube.com/watch?v=vLkPBj9ZaU0)  
[Google Developer : Web Fundamentals - Custom elements](https://developers.google.com/web/fundamentals/web-components/customelements?hl=ko)  
[Google Developer : Web Fundamentals - Shadow DOM](https://developers.google.com/web/fundamentals/web-components/shadowdom#styling)  