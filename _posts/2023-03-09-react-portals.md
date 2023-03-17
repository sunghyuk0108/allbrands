---
layout: single
title: "REACT Portals"
categories: "react"
tag: [react]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## REACT Portals

예를 들어 어떤 어플리케이션에서는 하나의 컴포넌트가 전체 어플리케이션 위쪽에 위치해 있지 않고 다른 컴포넌트 안에 깊이 중첩되어 있기도 하다.

예를 들어 모달창이 하나 있다고 가정해 보자 해당 모달의 경우 렌더링될 위치를 고려하여 작업되어야 하는데 portals를 사용하면 원하는 위치로 지정하여 컴포넌트를 이동시킬 수 있다.

아래 예제를 보면서 portals의 사용법을 익혀보자

포털에는 두 가지가 필요하다.\
(1) 컴포넌트를 이동시킬 장소가 필요하다.

(2) 그런 다음 컴포넌트에게 그 곳에서 포털을 가져야 한다고 알려줘야한다.

그 장소를 표시하기 위해 react 프로젝트 내의 public 폴더로 이동하여 index.html 파일을 연다.

#### 1.아래의 코드 블록처럼 보통 `<div id="root"></div>`외에 div에 id를 추가해서 해당 장소를 찾아올 수 있도록 지정한다.

*추가된 코드

```
<div id="backdrop_root"></div>
<div id="overlay_root"></div>
```

index.html 파일에서 컴포넌트를 이동시킬 장소를 생성해주었다.

#### 2.아래의 코드 블록처럼 ErrorModal.js 컴포넌트의 코드를 작성해 보자

*추가된 코드
```
const Backdrop = (props) => {
  return <div className={classes.backdrop} onClick={props.onConfirm} />;
};
//Backdrop 분리

const ModalOverlay = (props) => {
  return (
    <Card className={classes.modal}>
      <header className={classes.header}>
        <h2>{props.title}</h2>
      </header>
      <div className={classes.content}>
        <p>{props.message}</p>
      </div>
      <footer className={classes.actions}>
        <Button onClick={props.onConfirm}>Okay</Button>
      </footer>
    </Card>
  );
};
//ModalOverlay 분리
```

1번에서 추가된 backdrop_root, overlay_root 각각에 쉽게 포털시키기 위해 Backdrop, ModalOverlay 두개의 컴포넌트로 분리했다.

##### 3.그럼 기존의 ErrorModal 컴포넌트의 Fragment안에서는 무엇을 해야할까?

아직 JSX코드 내부에 있기 때문에 표현을 추가할 수 있다. 우선 react-dom을 import 하고 createPortal() 메서드를 활용하여 포털시켜보자.

*Fragment는 React v16에 추가된 기능
컴포넌트가 여러 엘리먼트를 return 할때 jsx규칙상 하나의 태그로 묶어서 return 해줘야 한다. 이때 fragment를 사용하면 dom에 별도의 노드를 추가하지 않고 여러자식을 그룹화 할 수 있다.

*추가된 코드
```
import ReactDOM from "react-dom";
//우선 react-dom을 import 한다.
```

우선 createPortal()메서드를 사용하기 위해 react-dom을 import 해준다.

*추가된 코드
```
const ErrorModal = (props) => {
  return (
    <Fragment>
      {ReactDOM.createPortal(
        <Backdrop onConfirm={props.onConfirm} />,
        document.getElementById("backdrop_root")
      )}
      ;{/* createPortal는 두개의 인수를 취함 */}
      {ReactDOM.createPortal(
        <ModalOverlay
          title={props.title}
          message={props.message}
          onConfirm={props.onConfirm}
        />,
        document.getElementById("overlay_root")
      )}
      ;{/* ModalOverlay에 여러가지 props를 전달함.*/}
    </Fragment>
  );
};
```

createPortal() 메서드는 두개의 인수를 취하는데

(1) 첫번째 인수는 렌더링되어야 하는 리액트 노드다.\
첫번째 인수는 렌더링 되는 리액트 노드인데 여기서 중요한 것은 JSX로 작성되어야 한다는 것이다. 이렇게하면 편리한게 props를 전달하기 편리해진다.Backdrop 컴포넌트의 onConfirm을 porps.onConfirm으로 접근하여 전달할 수 있고, ModalOverlay 컴포넌트의 경우에도 많은 porps.title등으로 접근하여 전달하기 편해진다.

(2) 두번째 인수는 렌더링 되어야할 포인터이다.\
이 요소가 렌더링되어야 하는 실제 DOM의 컨테이너를 가리키는 위치다.
이때 실제로 렌더링되어야 하는 요소를 선택하기 위해 DOM-API를 사용한다.
document.getElementById("아이디 이름");를 입력한다.
따라서 실제 HTML DOM 요소에 접근할 수 있다.


#### AddUser.js 컴포넌트의 코드

AddUser 컴포넌트의 코드는 전혀 건들지 않았다.
ErrorModal을 이전에  사용했던 것처럼 그대로 사용하면서 아무것도 바꾸지 않고 이전처럼 커뮤니케이션할 수 있다.
```
import React, { useState, Fragment } from 'react';

import Card from '../UI/Card';
import Button from '../UI/Button';
import ErrorModal from '../UI/ErrorModal';
import classes from './AddUser.module.css';

const AddUser = (props) => {
  const [enteredUsername, setEnteredUsername] = useState('');
  const [enteredAge, setEnteredAge] = useState('');
  const [error, setError] = useState();

  const addUserHandler = (event) => {
    event.preventDefault();
    if (enteredUsername.trim().length === 0 || enteredAge.trim().length === 0) {
      setError({
        title: 'Invalid input',
        message: 'Please enter a valid name and age (non-empty values).',
      });
      return;
    }
    if (+enteredAge < 1) {
      setError({
        title: 'Invalid age',
        message: 'Please enter a valid age (> 0).',
      });
      return;
    }
    props.onAddUser(enteredUsername, enteredAge);
    setEnteredUsername('');
    setEnteredAge('');
  };

  const usernameChangeHandler = (event) => {
    setEnteredUsername(event.target.value);
  };

  const ageChangeHandler = (event) => {
    setEnteredAge(event.target.value);
  };

  const errorHandler = () => {
    setError(null);
  };

  return (
    <Fragment>
      {error && (
        <ErrorModal
          title={error.title}
          message={error.message}
          onConfirm={errorHandler}
        />
      )}
      <Card className={classes.input}>
        <form onSubmit={addUserHandler}>
          <label htmlFor="username">Username</label>
          <input
            id="username"
            type="text"
            value={enteredUsername}
            onChange={usernameChangeHandler}
          />
          <label htmlFor="age">Age (Years)</label>
          <input
            id="age"
            type="number"
            value={enteredAge}
            onChange={ageChangeHandler}
          />
          <Button type="submit">Add User</Button>
        </form>
      </Card>
    </Fragment>
  );
};

export default AddUser;
```

아래 이미지를 보면 코드 작성 후 지정한 포인터로 렌더링 되는 것을 볼 수 있다.

![포인터에 렌더링된 이미지](/images/portals_img1.png "Optional title")

[REACT 공식 Portals 설명으로 이동](https://ko.reactjs.org/docs/portals.html#gatsby-focus-wrapper)

