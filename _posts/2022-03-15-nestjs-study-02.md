---
layout: post
title: "[NestJS 정리] 2. NestJS 설치 및 시작하기"
# subtitle: Each post also has a subtitle
categories: NestJS
# tags: [test]
---

## 1. Node.js 설치
NestJS는 Node.js를 편하게 사용하기 위한 프레임워크이므로 공식홈페이지에서 Node.js를 설치해준다. 

>https://nodejs.org/ko/
  
LTS 버전과 최신 버전이 있는데, 최신 버전에는 해결되지 않은 버그가 있을 수 있으니 **LTS버전**으로 받는것이 좋다!  


## 2. Node.js 설치 확인
``` bash
node -v
```
에러가 발생하지 않고 버전 정보가 출력되면 정상 설치가 된 것이다.


## 3. NestJS CLI 설치
NestJS를 이용하여 프로젝트를 시작할 때는 NestJS CLI 프로그램을 사용하는 것이 좋다.
```bash
npm i -g @nestjs/cli
```
만약 설치중에 permission 에러가 난다면 앞에 **sudo**를 붙여서 재실행 하면 된다.

## 4. CLI를 이용하여 NestJS 프로젝트 생성
```bash
# nest new {프로젝트 이름}
nest new project-name
```
  


>John Ahn 님의 유튜브 영상을 보고 정리한 내용입니다.  
문시 삭제 하겠습니다.  
  
https://www.youtube.com/watch?v=3JminDpCJNE&list=WL&index=3
