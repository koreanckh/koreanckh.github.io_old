---
layout: post
title: "[NestJS 정리] 4. NestJS를 배우기 전에 알아두면 좋을 것들"
# subtitle: Each post also has a subtitle
categories: NestJS
# tags: [test]
---

## 1. Node.js

- Node.js로 동작하기 위해 컴파일

NestJS는 Node.js를 기반으로 동작하는데, 정확하게는 Nest로 작성한 코드들을 Express나 Fastify 같은 Node.js 기반의 프레임워크에서 실행이 가능하도록 컴파일 해주는 역할을 하기 때문에 Node에 대한 이해가 필요하다. Node.js는 자바스크립트를 이용하여 서버를 구성할 수 있도록 만들었기 때문에 프론트와 백엔드에서 같은 언어를 사용하여 개발 할 수 있다는 것이 가장 큰 장점이다. NPM이라고 하는 패키지 관리 프로그램을 통해 관리한다.
  
- 특징

**단일 쓰레드에서 구동되는 non-blocking I/O 이벤트 기반의 비동기 방식**이다. 하나의 쓰레드에서 하나의 작업만 처리하게 되는데, 애플리케이션 단에서는 단일 쓰레드로 작동을 하지만 백그라운드에서는 쓰레드 풀을 구성하여 작업을 처리하고 있다. 또한 쓰레드 풀 관리 Node.js에 내장된 libuv에서 해주고 있기 때문에 개발하기에 한층 더 편안한 환경을 제공한다.  
Node.js는 앞의 작업이 끝날때까지 기다리지 않고(non-blocking) 비동기로 처리한다. 입력은 하나의 쓰레드에서 받지만 순서대로 처리하지 않고 먼저 처리된 결과를 이벤트로 반환해주는 방식이 Node.js를 구성하고 있는 단일 쓰레드 기반의 non-blocking 이벤트 기반 비동기 방식이다.

- Node.js의 장단점

위와 같은 방식은 서버의 자원에 크게 부하를 주지 않아 대규모 네트워크 애플리케이션을 개발하는데 적합하다. 하지만 단일 쓰레드 기반으로 동작하기 때문에 하나의 쓰레드에 문제가 생기면 애플리케이션 전체에 문제가 생길수 있게 된다. 또한 컴파일러의 언어 처리 속도에 비해 성능이 떨어진다는 단점도 있으나 V8엔진의 성능이 점점 좋아지고 JavaScript 또한 계속해서 발전하고 있다.


## 2. 이벤트 루프

> JavaScript가 단일 쓰레드 기반임에도 불구하고 Node.js가 non-blocking I/O작업을 수행하게 해주는 핵심 기능이라고 할 수 있다.  
(아직은 Node.js가 익숙하지 않아서 추후에 보완하기로 하고 패스..)

## 3. Typescript

> NestJS는 Typescript를 기본언어로 채택하고 있다.

Typescript는 마이크로소프트에서 개발한 언어이다. `tsc` 명령어를 이용하여 컴파일하여 Javascript 코드로 변환하는데, Javascript는 원래 타입이 없는 언어이기 때문에 컴파일 후에는 타입이 존재하지 않는다. 실제 개발시에는 VS Code 같은 IDE에서 문법 검사 및 에러 표시 기능이 있기 때문에 굳이 `tsc` 명령어를 사용하지 않아도 된다.

- 타입

boolean, null, undefined, number, bigint, string, symbol 의 기본타입 및 객체 타입을 지원하고 함수 타입, any/unkwon/nerver 같은 Typescript 의 특수한 타입 등이 존재한다. 자세한 사항은 Typescript를 따로 공부하여 습득해야 할 것 같다...


## 4. 데코레이터(Decorator)
> Python의 데코레이터나 Java의 annotation과 비슷한 역할.

Typescript에는 아래와 같이 여러 종류의 데코레이터가 존재하고 사용자가 임의로 정의하여 사용하는 것도 가능하다. 지금은 'Typescript에도 Java에서의 annotation 같은게 있구나' 하는 정도로 넘어가면서 차차 알아 가는 것이 좋다고 본다.

- 클래스 데코레이터
- 메서드 데코레이터
- 접근자 데코레이터
- 속성 데코레이터
- 매개변수 데코레이터(Parameter 데코레이터)

## 5. 백엔드 로드맵
<center>
    <img src="/assets/images/post/nestjs/nestjs-study-04-Roadmap.png">
</center>
* 출처 : https://roadmap.sh/backend  
