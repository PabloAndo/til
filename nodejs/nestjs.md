# NestJS

Official documentation : <https://docs.nestjs.com/>

#### Installation

Regular installation: `npm i -g @nestjs/cli`

TypeScript starter project: `git clone https://github.com/nestjs/typescript-starter.git project`

#### New project

`nest new project_name`

By default the app is running in **PORT = 3000**

## Controller

Official docs : <https://docs.nestjs.com/controllers>

Controllers are responsible for handling incoming **requests** and returning **responses** to the client. Frequently, each controller has more than ore route, and different routes can perform different actions.

To create a controller using the CLI: `$ nest g controller cats`

Basic controller example:

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'This action return all cats';
  }
}
```

#### Routing

The @Get() HTTP request method decorator tells Nest to create a handler for a specific endpoint for HTTP requests.
The handler could concatenate routes, for example @Get('wild') will point to: '/cats/wild

#### Request Object

Handlers often need access to the client **request** details. It can be accessed by adding @Req().

```typescript
import { Controller, Get, Req } from '@nestjs/common';
import { Request } from 'express';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Req() request: Request): string {
    return 'This action returns all cats';
  }
}
```

The request object represents the HTTP request and has properties for the request query string, parameters, HTTP headers, and body: <http://expressjs.com/en/api.html#req>

Nest provides decorators out of the box, for example: @Body() or @Query() to access the **request object properties**.

#### Status Code

Status code can be cahnge by adding the @HttpCode(...):

```typescript
@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```

#### Headers

Specify a custom response header:

```typescript
@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```

#### Route Parameters

Routes can accept **dynamic data** as part of t he request.

```typescript
@Get(':id')
findOne(@Param() params): string {
  console.log(params.id);
  return `This action returns a #${params.id} cat`;
}
```

### Database (TypeORM)

Official docs: <https://docs.nestjs.com/techniques/database>

## Providers

The main idea of a provider is that it can **inject** dependencies. A provider is a class annotated with an @Injectable() decorator.

To create a service using the CLI: `$ nest g service cats`

Example:

```typescript
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```

## Modules

Each app hast at least one module, a **root module**. The root module is the starting point Nest uses to build the application graph - the internal data structure Nest uses to resolve module and provider relationships an dependencies.

The @Module() decorator takes:

- providers : the providers that will be instantiated by the Nest injector and that may be shared at least across this module
- controllers : the set of controllers defined in this module which have to be instantiated
- imports : the list of imported modules that export the providers which are required in this module
- exports : the subset of providers that are provide by this module and should be available in other modules which import this module.

Modules helps us manage complexity and develop with **SOLID** principles. Keeping code organized and establishing clear boundaries.

To create a module using the CLI: `$ nest g module cats`

```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
})
export class CatsModule {}
```

Unlike in Nest, Angular providers are registered in the global scope. Once defined, they're available everywhere. Nest, however, encapsulates providers inside the module scope. You aren't able to use a module's providers elsewhere without first importing the encapsulating module.

When you want to provide a set of providers which should be available everywhere out-of-the-box (e.g., helpers, database connections, etc.), make the module global with the @Global() decorator.

```typescript
import { Module, Global } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Global()
@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
})
export class CatsModule {}
```

## Middleware

Middleware is a function which is called **before** the route handler. Middleware function have access to the **request** and **response** objects, and the next() middleware function in the app's req-res cycle.

Middleware functions can perform the following tasks:

- execute any code.
- make changes to the request and the response objects.
- end the request-response cycle.
- call the next middleware function in the stack.
- if the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging.

Example:

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: any, res: any, next: () => void) {
    console.log('Request...');
    next();
  }
}
```

Apply the middleware as follow, in the AppModule:

```typescript
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(LoggerMiddleware).forRoutes('cats');
  }
}
```

We can exclude middleware from particular routes:

```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude(
    { path: 'cats', method: RequestMethod.GET },
    { path: 'cats', method: RequestMethod.POST }
  )
  .forRoutes(CatsController);
```

#### Multiple Middlewares

`consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);`

#### Global Middleware

```typescript
const app = await NestFactory.create(AppModule);
app.use(logger);
await app.listen(3000);
```

## Exception Filters

Example:

```typescript
@Get()
async findAll() {
  throw new HttpException({
    status: HttpStatus.FORBIDDEN,
    error: 'This is a custom message',
  }, 403);
}
```

Response:

```JSON
{
  "status": 403,
  "error": "This is a custom message"
}

```

## Pipes

Pipes should implement the PipeTransform.

Pipes have two typical use cases:

- **transformation**: transform input data to the desired output
- **validation**: evaluate input data and if valid, simply pass it through unchanged; otherwise, throw an exception when the data is incorrect

In both cases, pipes operate on the **arguments** being processed by a **controller route handler**. Nest interposes a pipe just before a method is invoked, and the pipe receives the arguments destined for the method.

Pipes run inside the exceptions zone.

There are three pipes available right out-of-the-box:

- ValidationPipe
- ParseIntPipe
- ParseUUIDPipe

Every pipe has to provide the `transform()` method. This method has two parameters:

- value
- metadata

The `value` is the currently processed argumnet (before it is received by the route handling method). While `metadata` is its metadata. The metadata object has these properties:

```typescript
export interface ArgumentMetadata {
  type: 'body' | 'query' | 'param' | 'custom';
  metatype?: Type<any>;
  data?: string;
}
```
These properties describe the currently processed argument:
  - Type: Indicates wheter the argument is a body `@Body()`, `Query()`, `@Param()` or a custom parameter.
  - metatype: Provides the metatype of the argument, for example, `String`. 
  - data: the string passed to the decorator, for example `@Body('string')`. 