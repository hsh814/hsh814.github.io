---
title: "Haskell 개발 환경 설치하기"
excerpt: "하스켈 설치 및 기본 설정"
date: 2020-02-25 22:49:00 -0400
categories: 
  - haskell
tags:
  - haskell
  - stack

last_modified_at: 2020-02-25 22:49:00 -0400
---

# 하스켈 개발 환경 설치하기

last modified at: {{ page.last_modified_at }}

하스켈을 시작해보고 싶은 사람들을 위한 초기 설치 정보

## 목록
- [하스켈 개발 환경 설치하기(stack)].(#하스켈-개발-환경-설치하기)
- [vscode](#vscode)
- [intellij](#intellij)


## 1. 컴파일러

하스켈 컴파일러의 설치 방법

### 1.1. ghc

ghc = Glorious Glasgow Haskell Compilation System.

아치 리눅스에서는 

```
sudo pacman -S ghc
```

데이안에선

```
sudo apt install ghc
```

등으로 설치할 수 있다.

하지만 라이브러리가 포함되지 않기 때문에 Stack을 설치하는 것이 좋다.

### 1.2. Stack

스택은 standard hackage(haskell package)를 관리하는 시스템이다.

예전엔 하스켈 플랫폼을 썼으나, 요즘은 스택을 주로 사용한다.

본 문서도 스택을 기준으로 설명할 것이다.

- [reference](https://docs.haskellstack.org/en/stable/README/)

아치 리눅스에서는
```
sudo pacman -S stack
```

기타 리눅스에서는 이렇게 설치할 수 있다.

```
curl -sSL https://get.haskellstack.org/ | sh
```


## 2. Stack의 기본적인 사용법

```
stack new my-project
cd my-project
stack setup
stack build
stack exec my-project-exe
```

### 2.1. 프로젝트의 구조

`stack new`로 새로운 프로젝트를 만들 수 있다.

```
.
├── app
│   └── Main.hs
├── ChangeLog.md
├── LICENSE
├── my-project.cabal
├── package.yaml
├── README.md
├── Setup.hs
├── src
│   └── Lib.hs
├── stack.yaml
└── test
    └── Spec.hs
```

새 패키지를 추가하려면 `my-project.cabal`이라는 파일의 dependency에 적어주면 된다. 

`src` 아래에 새로운 모듈을 추가한다면 `my-project.cabal`의 exposed-module에 추가해주어야 한다.

### 2.2. 빌드

`stack build`로 프로젝트를 빌드 할 수 있다. 결과물은 `.stack-work` 안에 저장된다.

### 2.3. 실행

`stack exec project-name-exe`로 실행할 수 있다. `.stack-work`내의 실행 파일을 찾아 자동으로 실행시켜 준다.

### 2.4. ghci

`ghci`는 하스켈의 interactive 모드이다. 

`stack ghci`로 실행시킬 수 있다.

### 2.5. runhaskell

`stack runhaskell`로 실행시킬 수 있다.


## vscode

vscode는 굉장히 유용한 에디터이다.

```
sudo pacman -S code
```

하스켈을 위한 익스텐션으로는 haskero를 추천한다.

### 익스텐션: haskero

haskero는 코드 자동완성, 타입 후버링 등의 기능을 제공한다. 

- [refernece](https://gitlab.com/vannnns/haskero/blob/master/client/doc/installation.md)

stack이 설치되어야 한다.

```
cd ~
stack setup
stack install intero --copy-compiler-tool
cd /my/haskell/project
code .
```


## intellij

intellij IDEA는 유용한 IDE이다. 역시 intellij-haskell이라는 플러그인을 지원한다.

- [link](https://www.jetbrains.com/idea/download/#section=linux)

arch linux에서는 간단하게 설치할 수 있다.

```
sudo yay -S intellij-idea-community-edition
혹은 라이선스가 있다면
sudo yay -S intellij-idea-ultimate-edition
```

다른 배포판에서도 snappy를 이용하면 쉽게 설치할 수 있다.

```
sudo snap install intellij-idea-community
혹은 라이선스가 있다면
sudo snap install intellij-idea-ultimate
```

### 플러그인: Intellij-Haskell

마켓에서 haskell을 검색해서 설치하면 된다.

stack으로 만든 프로젝트를 선택해서 열면 아마 잘 되지 않을 것이다...

약간의 설정이 더 필요하다.

file > project settings > project sdk에 stack의 바이너리 파일 경로를 입력해야 하기 때문이다. 찾아서 입력해주면 된다.

아치의 경우 `/usr/bin/stack`이다.

그 후 기다리다 보면 인텔리제이에서 intero, stylish-haskell등을 자동으로 설치한다.

