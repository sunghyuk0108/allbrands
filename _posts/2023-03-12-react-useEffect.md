---
layout: single
title: "REACT useEffect"
categories: "react"
tag: [react]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## REACT useEffect

useEffect를 사용하는 이유는

useEffect를 사용하게 되면 useEffect에 전달된 함수는 화면에 렌더링이 완료된 후에 수행되게 될 것입니다.

예를들어 로그인 기능을 구현한다고 가정해보자.

#### 1. loginHandler 함수 실행될 때 localStorage를 활용하여 로그인 기능을 먼저 구현한다.

```
import React, { useState, useEffect } from "react";

import Login from "./components/Login/Login";
import Home from "./components/Home/Home";
import MainHeader from "./components/MainHeader/MainHeader";

function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

    const storedUserLoggedInInformation = localStorage.getItem("isLoggedIn");

  const loginHandler = (email, password) => {
    localStorage.setItem("isLoggedIn", "1");
    setIsLoggedIn(true);
  };

  return (
    <React.Fragment>
      <MainHeader isAuthenticated={isLoggedIn} onLogout={logoutHandler} />
      <main>
        {!isLoggedIn && <Login onLogin={loginHandler} />}
        {isLoggedIn && <Home onLogout={logoutHandler} />}
      </main>
    </React.Fragment>
  );
}

export default App;

```

loginHandler 함수가 작동 시 localStorage.setItem("키","값")이 설정되게끔 구현하였다.
localStorage의 값이 저장됐다면 setIsLoggedIn(true); 값으로 변경하여 로그인이 완료될 수 있게끔 적용했다.

#### 2. localStorage.getItem()을 활용하여 값을 가지고 있는지 확인 하고 로그인이 됐다면 상태값을 유지하자.

```
    const storedUserLoggedInInformation = localStorage.getItem("isLoggedIn");

    if (storedUserLoggedInInformation === "1") {
      setIsLoggedIn(true);
    }
```

이때 페이지를 새로고침하거나 다시 컴포넌트가 렌더링 된다면 localStorage의 정보가 삭제된다. 이때 localStorage.getItem()을 활용하여 localStorage에 값이 존재한다면 그 값을 변수에 담고 저장 후 해당 키의 값의 로그인 시 값과 같다면 setIsLoggedIn(true);로 값을 설정하여 로그인이 계속 유지될 수 있도록 작성해 보았다.
(이때 무한 루프를 경험할 수 있다...)
무한 루프가 발생하는 이유는 위의 코드를 보면 storedUserLoggedInInformation값이 === "1"과 같다면 계속해서 state설정 함수를 호출할 때마다 App컴포넌트가 다시 실행되기 때문에 무한 루프에 빠지게된다.

#### 3. 무한 루프가 발생하기 때문에 useEffect를 활용하여 방지할 수 있다.

```
  useEffect(() => {
    const storedUserLoggedInInformation = localStorage.getItem("isLoggedIn");

    if (storedUserLoggedInInformation === "1") {
      setIsLoggedIn(true);
    }
  }, []);
```

useEffect 사용 시 useEffect(실행할 함수, [의존성 배열명])으로 작성되며 기본적으로 동작은 모든 렌더링이 완료된 후에 수행되지만, 어떤 값이 변경되었을 때만 실행되게 할 수도 있다. 예를 들면 의존성이 배열에 입력된 값이 변경될 경우 가능하다. 또한 빈 배열을 작성한다면 해당 컴포넌트가 렌더링되고 한번만 실행된다.

----------------------------------------------------------

### useEffect의 주요내용

* 기본적으로 동작은 모든 렌더링이 완료된 후에 수행됩니다만, 어떤 값이 변경되었을 때만 실행되게 할 수도 있습니다.
(의존성이 변경된 경우 예를 들면 처음 사이트를 렌더링했을 때)

* effect를 수행하고 (mount를 하거나 unmount 할 때) 그것을 한 번만 실행하고 싶다면 두 번째 인자로 빈 배열([])을 전달할 수 있습니다.

* useEffect는 어떤 상태값이 바뀌었을 때 동작하는 함수를 작성할 수 있다.
두번째 인자인 의존성 배열에 넣는 값은 의존성 배열안에 넣은 값이 변경될 때만 useEffect의 함수가 작동되게할 수 있음.

* useEffect를 사용할 때, 주의할 점 useEffect 함수 내에서 state를 변경하면 무한 루프가 발생할 수 있다. 이를 방지하기 위해서는 useEffect 함수 내에서 state를 변경하지 않고, useEffect 함수가 의존하는 state를 dependencies (의존성) 배열에 포함해야 합니다.

* 종속성을 추가할 경우 effect 함수에서 사용하는 "모든 것"을 추가해야한다. 즉, 거기서 사용하는 모든 상태 변수와 함수를 포함합니다.\
맞는 말이지만 몇가지 예외가 있다. 
1. 상태 업데이트 기능을 추가할 필요는 없다. (state 상태 변경 메서드) react는 해당 함수가 절대 변경되지 않도록 보장하므로 종속성으로 추가할 필요가 없음
2. "내장" API또는 함수를 추가할 필요가 없다.
(fetch(), localStorage) 같은 것들 브라우저에 내장된 함수 및 기능, 전역적으로 사용한 것들을 추가할 필요가 없다.
3. 구성 요소 외부에서 정의된 변수 함수는 추가할 필요가 없음.

### useEffect의 내장기능 Cleanup 함수

useEffect를 사용하게되면 가끔 클린업 작업이 필요한 이펙트가 있을거다.
예를 들어 키가 입력될 때마다 기본적으로 state가 업데이트 되는 함수가 실행된다고 가정해보자.
그런 함수가 있다면 문제가 될 수 있다. 더 복잡한 작업을 한다고 가정해보자 예를 들어 벡엔드로 http 리퀘스트로 사용자 이름이 이미 사용 중인지 확인 등으로 리퀘스트를 매우 많이 보내야하는 상황을 만들면 안된다. (불필요한 네트워크 트래픽을 만들게됨.)
그래서 일정량의 키 입력을 수집하거나 키 입력 후 일정 시간 이후에 실행될 수 있기를 원한다고 생각하고 작업해 보자.

cleanup 사용법
useEffect 내부의 첫번째 인자 한수를 전달할 때 return을 사용하여 return 내부에 함수를 전달함. (익명 함수 일반 함수 모두 가능)

*예제 코드
```
  useEffect(()=> {
    const identifier = setTimeout(() => {
      console.log("checking form validity!");
      setFormIsValid(
        enteredEmail.includes('@') && enteredPassword.trim().length > 6
      );
    }, 1000);

    return () => {
      console.log("clean up");
      clearInterval(identifier);
    }
  }, [enteredEmail, enteredPassword]);
```



[REACT 공식 useEffect 설명으로 이동](https://ko.reactjs.org/docs/hooks-reference.html#useeffect)

