---
layout: post
title: Bootstrap
tags:
  - memo
  - TIL
  - web design
  - bootstrap
---
Bootstrap 자체만으로 디자인을 퉁치는 건 안 좋은 습관.  
단, Bootstrap을 통해 웹에서 자주 사용하는 component와 element들을 파악해놓자.  
나중에 소스코드 보면서 customize도 해서 사용해보자.  

```
npm install bootstrap
import 'bootstrap/dist/css/bootstrap.css'
// import 'bootstrap/dist/css/bootstrap.min.css'
```
react에서는 이런 식으로 간단하게 bootstrap을 불러올 수 있다.  

`Form, Input`  
form과 input은 bootstrap을 이용하면 상당히 편해진다.  
Validation 검사라던가, 그런 것들이 굉장히 유용하니 기억해뒀다가  
나중에 필요하면 공식문서를 잘 이용하도록 하자.  
`Pagination`  
`Breadcrumbs`  
`Alerts`  
`Label, Badge`  
`Thumbnails`  
웹 디자인을 하는 데에 있어서 굉장히 중요한 것 중 하나가 바로 로딩 속도.  
코드가 아무리 길어도 로딩속도가 이미지 한두장보다 느리진 않음.  
보여주려는 이미지는 300x300인데, 원본은 1280x960이라던가  
이러면 불러오는 데에 엄청난 시간 낭비. 개손해. 로딩 개오래걸림.  
`Progress bars`  
`List`  
`Table`  
레이아웃을 짤 때 테이블을 사용하는 경우가 있는데  
굉장히 안좋은 방법이라고 한다. 마크업 방식에 어긋난다네.  
`Embed`  
영상, 지도 등을 다른 사이트에서 불러오거나 할 때 사용.  
`Modal`  
`Tab`  
`Tooltip, Popover`  
툴팁은 마우스 올려놓으면 뜨는 거.   
팝오버는 마우스 클릭하면 설명이 뜨는 거.  
모바일에서는 mouseover가 없기 때문에 툴팁보다는 팝오버를 사용.  
`Accordion`  
`Carousel`  
 
[Bootstrap 한글문서](http://bootstrapk.com/components/)  