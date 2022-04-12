---
layout: post
title: "[NestJS 정리] 4. NestJS 인터페이스"
# subtitle: Each post also has a subtitle
categories: NestJS
# tags: [test]
---

## 1. 컨트롤러(Controller)
우리가 알고 있는(?) MVC 패턴의 그 Controller가 NestJS에서도 사용되고 동일하게 request/response의 역할을 한다.

### (1) 컨트롤러 생성
직접 코딩해도 되지만 nest CLI를 이용하면 간단하게 생성 가능하다.
```bash
# 프로젝트의 root에서 실행
nest g controller {생성할 컨트롤러 이름}
```
<center>
        <img src="/assets/images/post/nestjs/nestjs-study-05-controller.png">
</center>

cf) 아래와 같이 module, controller, service, entity, dto 코드와 테스트 코드를 자동 생성하는 명령어도 있다.
```bash
# 프로젝트의 root에서 실행
nest g resource {생성할 이름}
```

### (2) 라우팅(Routing)
- app.controller.ts
```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

localhost의 root 경로에 해당한다. 자바의 스프링을 배운적이 있기에 유사한 구조가 눈에 들어온다. `@Controller()`, `@Get()` 같은 데코레이터가 사용되고 특히 `@Contoller()`라는 데코레이터가 있어 이 클래스가 Conroller임을 알려주고 있다.  
(클래스의 이름에 *Controller로 네이밍 되어 컨트롤러가 아님 주의 ㅋㅋ)  
`@Get()`의 괄호 안에는 `/`(루트경로)가 생략되어 있고, 원하는 URI를 입력해주면 해당 요청을 처리하는 처리하는 할 수 있다. 


### (3) 와일드카드

라우팅 패스(Routin path)는 와일드카드`*`를 사용하여 정의할 수 있다. 뿐만 아니라 `?`, `+`, `()` 같은 정규식 활용도 가능하다.  

```typescript
  @Get("/*company")
  getCompany() : string {
    return this.appService.getHello();
  }
```

위와 같이 라우팅 패스를 `@Get("/*company")`라고 정의할 경우 `*`에 어떤 문자열이 오더라도 상관없이 처리하겠다는 의미가 됩니다. 따라서 `http://localhost:3000/company`나 `http://localhost:3000/goodcompany`, `http://localhost:3000/badcompany` 등등 한번에 처리 가능하게 된다.  
와일드카드는 문자열의 앞이나 중간, 끝 위치에 상관없이 사용 가능하다.


### (4) 요청 객체(Request Object)

라우팅 패스를 잘 구성해두어도 좀 더 정확한 처리나 데이터 작업 등의 작업을 위해 클라이언트에서 서버로 요청을 보낼떄 서버에서 필요로 하는 정보를 담아서 전달해야 한다. Java에 `@RequestBody`가 있듯이 Typescript에는 `@Req()` 데코레이터를 사용하여 클라이언트로부터 요청받은 데이터를 핸들링 할 수 있게된다.

```typescript
/**
 *  app.controller.ts 
 */

import { Controller, Get, Req } from "@nestjs/common";
import { UsersService } from "./users/users.service";
import { Request } from "express";

@Controller()
export class AppController {
  constructor(private readonly usersService: UsersService) {  }

  @Get('/user')
  getUser(@Req() req: Request): string {
    console.log("Request >> ", req);
    return this.usersService.findAll();
  }

}
```

`@Req`를 받아오는 `Request` 객체는 `Express`의 객체이므로 [Express문서](https://expressjs.com/en/api.html#req)를 보며 더 자세한 내용도 익혀보자.  

cf)  
`@Query()`, `@Param(key?: string)`, `@Body()`등등의 데코레이터를 이용하여 데이터를 좀 더 쉽게 받아올 수도 있다.


### (5) 응답

앞서 `nest g resource users` nest CLI를 이용하여 간단하게 REST API를 생성하였다. 그 후에 서버를 실행하면 아래와 같은 로그들이 찍힌다.
```text
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [NestFactory] Starting Nest application...
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [InstanceLoader] AppModule dependencies initialized +16ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [InstanceLoader] UsersModule dependencies initialized +0ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RoutesResolver] AppController {/}: +15ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RouterExplorer] Mapped {/user, GET} route +1ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RoutesResolver] CommonController {/common}: +0ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RoutesResolver] UsersController {/users}: +0ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RouterExplorer] Mapped {/users, POST} route +0ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RouterExplorer] Mapped {/users, GET} route +1ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RouterExplorer] Mapped {/users/:id, GET} route +0ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RouterExplorer] Mapped {/users/:id, PATCH} route +0ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [RouterExplorer] Mapped {/users/:id, DELETE} route +0ms
[Nest] 95082  - 04/12/2022, 3:25:23 PM     LOG [NestApplication] Nest application successfully started +1ms

```

특히 RouterExplorer에 찍힌 로그들을 보면 Controller에 정의하여 매핑 된 routing path들이 찍힌다.  

- @Res() 데코레이션 사용  

정상적인 처리가 되면 서버에서는 200대 응답을 보내준다. POST의 경우에는 `201`응답을 하고 나머지 GET, PUT, DELETE 등은 `200`응답을 보내온다. 또한 응답 데이터가 객체인 경우 객체를 자동으로 JSON타입으로 변환하여 전달하지만 Express의 `@Res()` 데코레이션을 활용해서도 리턴이 가능하고, 정상 처리시 응답 상태 코드(status)를 설정해줄수도 있다. 
```typescript
/**
 * @Res() 예시
 */

@Get('findAll')
getFindAll(@Res() res) {
const users = this.usersService.findAll();

return res.status(200).send(users);
}
```

- @HttpCode({상태코드})

POST의 기본 응답 상태값은 201인데 다른 값으로 바꿔주고 싶다면? NestJS에서 제공하는 `@HttpCode`데코레이션을 이용하여 재설정해보자!
```typescript
/**
 * HttpCode() 예시
 */

  @HttpCode(200)
  @Post('updateUser')
  updateUser(@Param('id') id : number, @Body() updateUserDto: UpdateUserDto) {
    this.usersService.update(id, updateUserDto);
  }
```

200, 201 응답 상태 코드 이외에도 다양한 상태 코드와 의미들이 존재한다. 좀 더 나이스한 서비스를 위해 [MDN WEB DOCS](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/100)에서 제공하는 문서륾 확인해보고 적절하게 해주자~!


### (6) 헤더
NestJS에서 응답시 제공하는 헤더도 자동으로 구성하고 있으나 사용자에 맞게 커스텀이 가능하다.

```typescript
/**
 * Header 설정 예시
 */

  @Header("Custom-Head","Header Test")
  @HttpCode(200)
  @Post('updateUser/:id')
  updateUser(@Param('id') id : number, @Body() updateUserDto: UpdateUserDto) {
    return this.usersService.update(id, updateUserDto);
  }
```
HttpCode 설정을 예시로 들어둔 코드에 `@Header("Custom-Head","Header Test")`만 추가하였다.
- before

<center>
        <img src="/assets/images/post/nestjs/nestjs-study-05-header-before.png">
</center>

- after

<center>
        <img src="/assets/images/post/nestjs/nestjs-study-05-header-after.png">
</center>

> @Header() 데코레이션과 마찬가지로 @Res() 객체를 이용하여 res.header() 메서드 사용으로 커스텀 가능하다.


### (7) 리다이렉션(Redirection)
클라이언트의 요청에 대한 응답 이후 다른 페이지로 이동해야 하는 경우가 많은데 `@Redirect()`데코레이터를 이용하면 간편하게 설정이 가능하다. 첫번째 파라미터에는 리턴할 URI, 두번째 파라미터는 응답 코드값을 넣어주면 된다. 로그인 이후 다른 페이지로 리다이렉트 된다고 느낌으로 구성해보았다.
```typescript
/**
 * Redirection 예시
 */

  @Redirect('https://google.com', 302)
  @Get('login/:id')
  login(@Param('id') id : number, @Body() updateUserDto: UpdateUserDto) {
    return this.usersService.login(id, updateUserDto);
  }
```


### (8) 라우트 파라미터
Routing path에 단순하게 서비스 하기 위한 용도로 매핑하는 것 보다 라우트 파라미터를 이용하여 데이터를 주고 받을 수 있다.
```typescript
@Get(":id/board/:category/:number")
getBoard(@Param("id") id: number
    , @Param("category") category: string
    , @Param("number") number: number) {
return `id : ${id} , category : ${category} , number : ${number}`;
}
```
`@Get(":id/board/:category/:number")`세팅에 보면 콜론`:`을 사용하여 라우트 파라미터를 전달 받을수 있다. 예를들어 `http://localhost:3000/1234/board/IT/17`라고 요청이 온다면 `id : 1234 , category : IT , number : 17`로 매핑된다.  

<br>

위와 같이 사용시 파라미터가 많은 경우 한번에 받아오는 것도 가능하다.
```typescript
@Get(":id/board/:category/:number")
getBoard(@Param() params : {[key : string] : string}) {
  return `id : ${params.id} , category : ${params.category} , number : ${params.number}`;
}
```


### (9) 하위 도메인 라우팅
> 하위 도메인과 원래 도메인에서 다르게 처리가 되도록 구성 하고 싶은 경우

구글은 `http://google.com`로 접속 할 경우 검색할 수 있는 페이지가 뜨지만 `http://support.google.com`로 접속하면 고객센터 페이지가 뜨게 된다. support는 하위 도메인, google은 주 도메인, com은 최상위 도메인(TLD)라고 한다.  

#### a. 하위 도메인을 위한 컨트롤러 생성(nest CLI 이용)

```bash
nest g co support
```

#### b. 엔드 포인트 우선순위 수정
같은 엔드포인트가 있다면 SupportController가 먼저 처리될 수 있도록 순서를 변경해준다.
```typescript
/**
 * app.module.ts
 */

@Module({
  imports: [UsersModule],
  controllers: [SupportController, AppController, CommonController],
  providers: [AppService, UsersService],
})
export class AppModule {}
```

#### c. 하위 도메인 컨트롤러 수정
```typescript
import { Controller, Get } from "@nestjs/common";

@Controller({host: 'support.localhost'})
export class SupportController {
  @Get() // 기존에 app.controller.ts에도 존재하는 루트 경로의 루트 패스
  help() : string {
    return 'Can I Help You?';
  }
}
```