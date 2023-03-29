---
layout: single
title: "GIT"
categories: "GIT"
tag: [GIT, GIT HUB]
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## GIT으로 협업하기

만약 내가 만든 코드의 버전을 잃어버린다면 어떡하지? 라는 걱정이 될 수 있다.
협업의 시작은 백업으로 시작된다.

예를 들어 현재 내 컴퓨터를 local repository로 가정하고 백업을 할 repository를 remote repository로 가정해보자.
현재 local에서 romote로 옮긴다면 push 반대로 remote에서 local로 저장한다면 pull이라고 정의한다.

push, pull을 이용하여 협업가능.

### github.com

github.com을 활용하여 백업을 진행해보자.

* git client에는 git cli, source tree, vscode git 등이 있음.
* git server에는 github.com, gitlab.com, bitbucket.com 등이 있음.

1. github.com에서 New repository 생성.
생성 시 저장소의 이름과 Public 또는 Private 둘 중 하나로 설정
2. branch란 우선 해당 저장소의 최신버전이 누군지를 가르킨다고 생각하면됨 완전한 정의는 아님.
3. 터미널에 로그를 보면 초록색으로 master는 local 저장소의 마스터이고 빨간색으로 표시된 origin/master는 remote 저장소의 마스터다. log에서 마지막 v1은 버전을 가르킴.



