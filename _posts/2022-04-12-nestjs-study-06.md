---
layout: post
title: "[NestJS 정리] 6. NestJS 프로바이더(Provider)"
# subtitle: Each post also has a subtitle
categories: NestJS
# tags: [test]
---

## 1. 프로바이더(Provider)
컨트롤러는 클라이언트의 요청을 받고 응답 해주는 역할을 하고 있는데, 요청과 응답사이에 이루어지는 <u>비즈니스 로직</u>을 어떻게 구성하였는지가 중요하다고 할 수 있다. 이렇듯 비즈니스 로직을 수행하는 역할을 하는 것을 프로바이더라고 한다. 프로바이더는 서비스(Service), 레포지토리(Repository), 팩토리(Factory), 헬퍼(helper) 등등 다양한 형태로 구현할 수 있고 의존성을 주입하여 사용 할 수 있는데, NestJS에서도 의존성 주입을 위한 라이브러리를 제공하고 있다.  
프로바이더의 개념은 NestJS의 핵심적인 콘셉이라 할 수 있다. 프로바이더는 의존성을 주입할 수 있다는 것이 메인인데 

<br>

```typescript
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Patch(':id')
  update(@Param('id') id: string, @Body() updateUserDto: UpdateUserDto) {
    return this.usersService.update(+id, updateUserDto);
  }
}
```
컨트롤러는 클라이언트에 대한 요청과 응답을 담당할 뿐 비즈니스 로직까지 가지고 있는 것은 아니다. 비즈니스 로직은 UsersService에 구성을 하고 UsersController에서 이를 주입받아서 비즈니스 로직을 실행하도록 하여야 한다.

<br>

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class UsersService {
  update(id: number, updateUserDto: UpdateUserDto) {
    return `This action updates a #${id} user`;
  }
}
```
`@Injectable()`데코레이터를 선언해주면 다른 컴포넌츠에서 이를 주입하여 사용할 수 있는 프로바이더가 된다. 디폴트로 Singleton Instance가 생성되는데 설정에 따라 변경 가능하다.


## 2. 프로바이더 등록과 사용
### (1) 프로바이더 등록
`@Intejtable()`데코레이터를 이용해 프로바이더로 만들었다면 이것을 사용하기 위해서는 모듈 파일에 추가를 해주어야 한다.
```typescript
### app.module.ts

@Module({
  ...,
  providers: [AppService, UsersService],
})
export class AppModule {}

```

### (2) 프로바이더 사용 - 속성(propterty) 기반 주입
생성자를 통해 프로바이더를 주입받는 방법 외에 <u>상속 관계를</u> 통해 사용하는 방법이 있다. 자식 클래스에서 부모 클래스의 메서드를 호출할 때는 super()를 이용하여야 한다. `@Inject()`데코레이터를 사용하여 주입한다.
```typescript
@Injectable()
export class UsersService {

  @Inject('RequestService') private requestService : RequestService;

  login(id: number, updateUserDto: UpdateUserDto) {
    this.requestService.getMsg('dd');
    return `This action login a #${id} user`;
  }
}
```
`@Inject()`데코레이터의 인자에는 타입, 클래스명, 문자열, 심볼을 사용할 수 있다.  

> 상속관계의 경우에는 property 기반의 주입을 해주고, 상속관계가 아닌 경우라면 생성자 기반의 주입을 해주는 것이 좋다.


<br><br><br>

---
> Wikidocs에 올라온 "[NestJS로 배우는 백엔드 프로그래밍](https://wikidocs.net/book/7059)" 책을 보며 공부한 내용을 정리하였습니다.  
문제시 삭제 하겠습니다.
