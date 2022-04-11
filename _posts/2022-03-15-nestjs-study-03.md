---
layout: post
title: "[NestJS 정리] 3. NestJS 애플리케이션 예제 만들기"
# subtitle: Each post also has a subtitle
categories: NestJS
# tags: [test]
---

## 1. 프로젝트 생성
```bash
# 원하는 디렉토리로 이동하여 실행

# 1. 프로젝트 폴더 생성
mkdir nestjs-board-app

# 2. 생성한 폴더로 이동
cd nestjs-board-app

# 3. CLI를 이이용하여 프로젝트 생성
nest new ./
```

## 2. 프로젝트 구성
```
├── README.md
├── nest-cli.json
├── node_modules
├── package-lock.json
├── package.json
├── src
│   ├── app.controller.spec.ts
│   ├── app.controller.ts
│   ├── app.module.ts
│   ├── app.service.ts
│   └── main.ts
├── test
│   ├── app.e2e-spec.ts
│   └── jest-e2e.json
├── tsconfig.build.json
└── tsconfig.json
```
### (1) .eslintrc.js
개발자들이 특정한 규칙을 가지고 깔끔하게 코딩을 할 수 있도록 도와주는 라이브러리.  
ex) 타입스크립트를 올바르게 쓸 수 있도록 가이드라인 및 문법 오류 체크 등등.

### (2) .prettierrc
주로 코드의 형식을 맞추는데 사용.
큰따옴표("")를 사용할 것인지 작은따옴표('')사용 할 것인지 등등 코드의 코맷 형식을 정의하는 역할.

### (3) nest-cli.json
nestjs 프로젝트의 특정 설정을 위한 파일.

### (4) package.json
스프링의 pom.xml과 비슷한 역할.  
- "scripts" : `npm run` + "scripts" 이하에 정의된 명령어를 이용하여 실제로 실행할 명령어를 정의할 수 있다.  
ex) `npm run start` 명령어를 입력할 경우 실제 실행되는 것은 `nest start` 이다.
```json
 "scripts": {
    ...
    "start": "nest start",
    "start:dev": "nest start --watch",
    "start:debug": "nest start --debug --watch",
    "start:prod": "node dist/main",
    ...
  }
```
- "dependencies" : 패키지가 의존하고 있는 다른 패키지의 정보.  
- "devDependencies" : "dependencies"와 같은 기능을 하나 개발환경에서만 필요한 패키지들을 기술하는 곳으로 실제 오픈시에 사용할 패키지는 여기에 설치하지 않아야 한다.


### (5) package-lock.json
`npm install` 명령어를 실행하고 나면 `node_modules` 폴더가 생기면서 `package-lock.json` 파일도 같이 생성되고, `package.json` 파일을 업데이트 할 때마다 같이 자동으로 업데이트가 된다.  
`npm install` 명령어를 실행할 떄 `package-lock.json` 파일을 참조하여 패키지들을 설치하기 때문에 팀원들과 레포지토리 관리시 같이 공유하여야 한다.

### (6) tsconfig.json
타입스크립트 컴파일 방법에 대한 설정.

### (7) tsconfig.build.json
tsconfig.json 파일의 연장선으로 build시에 필요한 설정들을 명시.  
ex) "exclude" 값으로 제외할 파일도 설정 가능.

### (8) src 폴더
프로그램을 실행하기 위한 대부분의 로직.  
main.ts(앱을 생성하고 실행), app.module.ts(앱 모듈을 정의)

### (9) src/main.ts
```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
기본으로 설정된 3000 포트로 서버가 실행된다.

## 3. 프로젝트 실행
package.json내부에 정의된 스크립트 확인 후 npm이나 yarn 등을 사용하여 실행.  

```bash
# npm실행 예시
npm run start:dev
```


>John Ahn 님의 유튜브 영상을 보고 정리한 내용입니다.  
문시 삭제 하겠습니다.  
https://www.youtube.com/watch?v=3JminDpCJNE&list=WL&index=3
