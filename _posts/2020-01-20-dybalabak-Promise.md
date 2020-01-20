---
# 포스트 제목을 기재합니다.
title:  "Angular Constructor VS ngOnInit " 
# 저자를 입력합니다. 
author: "dybalabak"
# 카테고리를 입력합니다.
categories: ["Angular"]
# 태그를 입력합니다.
tags: ["ETC"]
last_modified_at: 2020-01-20T11:15:00-12:00
---
# Contructor VS ngOnInit
![posting]({{site.baseurl}}/assets/images/dybalabak/posting.png)

필자는 2학년을 수료하고 학교앞에서 실습을 진행중이다.

Angular에 대한 기초지식이 많이 부족한 상태에서,

Contructor 와 ngOnInit이 동일한 class 안에서 양립함에 있어
불편함을 밤세 참을 수 없었다 **(학점이 낮은 이유)**

설명에 앞서 **Angular life cycle hook**에 대한 지식을 선수하면 조금 더 수월하게 이해 수 있다.

위 첨부된 사진을 보면, **대충** Constructor에서 어떠한 Dependency Injection이
이루어 지고, JAVA의 Baseball baseball = new Baseball()의 향기가 강하게 풍긴다.

또, **대충** ngOnInit에서 해당 Component가 필요한 작업을 행함을 강하게 느낄 수 있다.

Constructor 와 ngOnInit에 대해 조금 더 자세하게 알아보자.




## Constructor란?


위 첨부된 사진에서,
Constructor는 typescript class의 predefined default method 이다.

딱히, Angular 와 Constructor 사이에 특별한 관계는 없다.

Angular는 Constructor의 parameter를 analyse하고,
해당 Constructor parameter type과 match되는 provider를 찾아준다.
그리고 resolve해주어 Constructor에게 넘겨준다.

일반적으로, Constructor에서 컴포넌트간 의존성을 주입하거나,변수를 define/initialize 해준다. 

**한마디로 쉽게요약하면, Constructor는 컴포넌트를 initialze하는데 사용되어진다.**





## ngOnInit이란?


**Angular life cycle hooks**중에 하나인 ngOnInit은,

Angular가 컴포넌트 초기화를 완료했다는 점을 전달하기 위해 존재한다.


**한마디로 쉽게요약하면,Constructor 상에서도 work를 수행할 수 있지만,
binding related work는 ngOnInit상에서 수행함이 바람직하다.**








