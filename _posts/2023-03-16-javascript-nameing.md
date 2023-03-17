---
layout: single
title: "javascript 식별자 네이밍 규칙"
categories: "javascript"
tag: [javascript]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## javascript 식별자 네이밍 규칙
식별자란 어떤 값을 구별해서 식별해낼 수 있는 고유의 이름이다.

아래 내용은 네임스페이스, 모듈, 함수, 변수등의 이름을 작성할 때 모두를 포함하고 있다.

* 식별자는 특수문자를 제외한 문자, 숫자, 언더스코어(_), 달러($)를 포함할 수 있다.
* 단, 식별자는 특수문자를 제외한 문자, 언더스코어(_), 달러 기호($)로 시작해야 한다.
* 예약어는 식별자로 사용할 수 없다.\
(예약어란 프로그래밍 언어에서 사용되고 있거나 사용될 예정인 단어를 말한다.)

* 단어를 생략하거나 약어를 사용하지 않는다. 단, HTML, URL 등과 같은 범용적인 약어는 사용할 수 있다. 약어 사용시에는 모두 대문자로 작성.
* 특수 문자는 가급적 사용하지 않지만 상수의 이름에서 단어를 구분하기 위한 용도와 Private 지시자 표시를 위하여 언더스코어(_)를 사용한다.

### 4가지 유형의 네이밍 컨벤션
#### 카멜케이스(camelCase)
```
const firstName;
```

#### 스네이크 케이스(snake_case)
```
const first_name;
```

#### 파스칼 케이스(PascalCase)
```
const FirstName;
```

#### 헝가리언 케이스 (typeHungarianCase)
```
const strFistName; // type + identifier
const $elem = document.getElementById('myId'); // DOM 노드
const observable$ = fromEvent(document, 'click'); // RxJS 옵저버블
```

-javascript deep dive\
-[공통 네이밍 규칙](https://github.com/naver/yobi/blob/master/docs/ko/technical/javascript-naming-convention.md)
참고하여 작성함.