# **Nombre:** Edier Dario Bravo Bravo
## **Nombre de la asignatura:** Electiva Desarrollo de Software - Web Semántica loT
## **Fecha de realización:** 25 de Noviembre del 2022


<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[circleci-image]: https://img.shields.io/circleci/build/github/nestjs/nest/master?token=abc123def456
[circleci-url]: https://circleci.com/gh/nestjs/nest

  <p align="center">A progressive <a href="http://nodejs.org" target="_blank">Node.js</a> framework for building efficient and scalable server-side applications.</p>
    <p align="center">
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/v/@nestjs/core.svg" alt="NPM Version" /></a>
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/l/@nestjs/core.svg" alt="Package License" /></a>
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/dm/@nestjs/common.svg" alt="NPM Downloads" /></a>
<a href="https://circleci.com/gh/nestjs/nest" target="_blank"><img src="https://img.shields.io/circleci/build/github/nestjs/nest/master" alt="CircleCI" /></a>
<a href="https://coveralls.io/github/nestjs/nest?branch=master" target="_blank"><img src="https://coveralls.io/repos/github/nestjs/nest/badge.svg?branch=master#9" alt="Coverage" /></a>
<a href="https://discord.gg/G7Qnnhy" target="_blank"><img src="https://img.shields.io/badge/discord-online-brightgreen.svg" alt="Discord"/></a>
<a href="https://opencollective.com/nest#backer" target="_blank"><img src="https://opencollective.com/nest/backers/badge.svg" alt="Backers on Open Collective" /></a>
<a href="https://opencollective.com/nest#sponsor" target="_blank"><img src="https://opencollective.com/nest/sponsors/badge.svg" alt="Sponsors on Open Collective" /></a>
  <a href="https://paypal.me/kamilmysliwiec" target="_blank"><img src="https://img.shields.io/badge/Donate-PayPal-ff3f59.svg"/></a>
    <a href="https://opencollective.com/nest#sponsor"  target="_blank"><img src="https://img.shields.io/badge/Support%20us-Open%20Collective-41B883.svg" alt="Support us"></a>
  <a href="https://twitter.com/nestframework" target="_blank"><img src="https://img.shields.io/twitter/follow/nestframework.svg?style=social&label=Follow"></a>
</p>
  <!--[![Backers on Open Collective](https://opencollective.com/nest/backers/badge.svg)](https://opencollective.com/nest#backer)
  [![Sponsors on Open Collective](https://opencollective.com/nest/sponsors/badge.svg)](https://opencollective.com/nest#sponsor)-->

## License
Nest is [MIT licensed](LICENSE).

# Requisitos

- Haber desarrollado la practica 2.

# Desarrollo

Recordar que el tema elegido en la practica 2 fue de tiquetes de vuelos de avion.

## Arquitectura Hexagonal

1. Para iniciar esta practica se crea una nueva rama denominada **hexagonal**, en la cual se van a guardar el proceso de esta practica.

```
git checkout -b hexagonal master
```

Para tener la estructura hexagonal se deben hacer los siguientes cambios en la estructura del codigo, donde **nombre_entidad** es **ticketfull**.

- src
  - <nombre_entidad>
    - adapters
      - controllers
        - <nombre_entidad>.controller.ts
      - repositories
        - <nombre_entidad>.repository.ts (nuevo archivo)
    - domain
      - models
        - <nombre_entidad>.model.ts (nuevo archivo)
      - services
        - <nombre_entidad>.service.ts

Ademas de mover los archivos **app.controllers** a la carpeta **controllers** y el archivo **app.service.ts** a la carpeta **service**, no olvidar de cambiarle el nombre de la clase de estos dos archivos.

3. Por otro lado se tiene la carpeta **models**, la cual se usa para modelar los datos y de esta manera agregar nuevos roles con sus respectivas entidades. Dentro de esta carpeta se encontraran dos archivos: 

El archivo **ticketfull.model.ts** que extiende la funcionalidad del modelo tiene el siguiente contenido.

```
import { Ticket } from "./ticket";

export class TicketFull extends Ticket{
    returndate: String
}
```

El archivo **ticket.ts**  que sera el modelo para futuros roles tiene el siguiente contenido.

```
export abstract class Ticket{
    passenger_name: string;
    source: string;
    destination: string;
    goingdate: string;
    flight: string 
}
```
4. Se crea un nuevo archivo denominado **ticketfull.service.ts**   dentro de la carpeta **service** con el objetivo de migrar las funcionalidades que antes el controlasdor tenia. El contenido de este archivo queda.

```
import { Injectable } from '@nestjs/common';
import {TicketFull} from '../models/ticketfull.model'

@Injectable()
export class TicketFullService {
  cambiarRegreso(id: number, regreso: string) {
    throw new Error('Method not implemented.');
  }
  private ticketfull: TicketFull[] = [{
    passenger_name: "Edier",
    source: "Popayan",
    destination: "Bogota",
    goingdate: '10-10-2022',
    flight: "K34HT8",
    returndate: '11/10/2022'
  }]

  public listar() : TicketFull[] {
    return this.ticketfull
  }

  public crear(tiketefull: TicketFull): TicketFull {
    this.ticketfull.push(tiketefull);
    return tiketefull;
  }

  public modificar(id: number, tiketefull: TicketFull): TicketFull {
      this.ticketfull[id] = tiketefull
      return this.ticketfull[id];
  }

  public eliminar(id: number): boolean {
    const totalTiketesAntes = this.ticketfull.length;
    this.ticketfull = this.ticketfull.filter((val, index) => index != id);
    if(totalTiketesAntes == this.ticketfull.length){
      return false;
    }
    else{
      return true;
    }
  }

   public cambiarReturno(id: number, returnDate: string): TicketFull {
      this.ticketfull[id].returndate = returnDate;
      return this.ticketfull[id];
   }

}
```
5. Dentro del controlador (**ticketfull.controller.ts**) se hace la implementacion sel servicio. Dentro de este es importante corregir errores para lo cual se agregan bloques **try/catch**. El controlador queda asi.

```
import { Body, Controller, Delete, Get, Param, Patch, Post, Put } from '@nestjs/common';
import { TicketFullService as TicketFullService } from '../../domain/services/ticketfull.service';

import {TicketFull} from '../../domain/models/ticketfull.model';

const errReturn = (e: Error, message: string) => {
  return {
    message: message,
    error: e
  }
}

@Controller()
export class TicketFullController {
  constructor(private readonly tiketeService: TicketFullService) { }

  @Get()
  getHello() {
    try{
      return this.tiketeService.listar();
    }
    catch(e){
      return errReturn(e, "Error al listar tiketes");
    }
  }

  @Post()
  crear(@Body() datos: TicketFull) {
    try{
      return this.tiketeService.crear(datos);
    }
    catch(e){
      return errReturn(e, "Error al crear tikete");
    }
  }

  @Put(":id")
  modificar(@Body() datos: TicketFull, @Param('id') id: number) {
    try{
      return this.tiketeService.modificar(id, datos);
    }
    catch(e){
      return errReturn(e, "Error al modificar tikete");
    }
  }

  @Delete(":id")
  eliminar(@Param('id') id: number) {
    try{
      return this.tiketeService.eliminar(id);
    }
    catch(e){
      return errReturn(e, "Error al eliminar tikete");
    }
  }

  @Patch(":id/regreso/:regreso")
  cambiarRegreso(@Param('id') id: number, @Param('regreso') regreso: string) {
    try{
      return this.tiketeService.cambiarRegreso(id, regreso);
    }
    catch(e){
      return errReturn(e, "Error al modificar fecha de regreso del tikete");
    }
  }
}
```

Hasta este punto la practica queda como se obserba en este [Link](https://github.com/edierbra/Practicas_IoT_Edier/blob/6f89a3c6aaffe1e7c313311c3f2bb8604b9e167b/src/ticketfull/adapters/controladores/ticketfull.controller.ts)

6. Es importante aplicar el principio SOLID, el cual define los siguientes principios:

 - Single Responsability Principle (Principio de Responsabilidad Única): Una clase debe tener una única responsabilidad y debe estar abierta a extensión pero cerrada a modificación.

 - Open-Closed Principle (Principio de Abierto-Cerrado): Las entidades de software (clases, módulos, funciones, etc.) deben estar abiertas a la extensión pero cerradas a la modificación.

 - Liskov Substitution Principle (Principio de Sustitución de Liskov): Las entidades de software (clases, módulos, funciones, etc.) deben ser sustituibles por instancias de sus subtipos sin alterar la correctitud del programa.

 - Interface Segregation Principle (Principio de Segregación de Interfaces): Las interfaces de software (clases, módulos, funciones, etc.) deben ser lo más pequeñas posibles.

 - Dependency Inversion Principle (Principio de Inversión de Dependencias): Las entidades de software (clases, módulos, funciones, etc.) deben depender de abstracciones y no de implementaciones.

## Implementar Seguridad

Para implementar un sistema de autentificacion y autorizacion se realoizan los siguientes pasos.

1. se instalan los paquetes necesarios que permitiran aplicar seguridad en este sistema.

```
npm install --save @nestjs/passport passport passport-local
npm install --save-dev @types/passport-local
```

2. **NestJS** permite la autenticación, por lo cual dentro de la carpeta del proyecto se crea un modulo de autenticacion usando los siguientes comandos.

```
nest g module auth
nest g service auth
```

![nestAuth](https://github.com/edierbra/Practicas_IoT_Edier/blob/main/Practica_3/Images/nestAuth.png?raw=true)

Acontiniacion se podra ver una carpeta llamada **auth** dentro de **src** y contendra los archivos **auth.module.ts**, **auth.service.ts** y **auth.service.spec.ts**. Este ultimo contiene las pruebas unitarias del servicio, el cual no se utilizara.

![Archivos carpeta auth](https://github.com/edierbra/Practicas_IoT_Edier/blob/main/Practica_3/Images/auth.png?raw=true)

3. **NestJS** tambien permite crear un modulo para gestionar usuarios, para lo cual se ejecutan los siguientes comandos dentro de la carpeta del proyecto. Esto creara una nueva carpeta llamada **users** dentro de **src**

```
nest g module users
nest g service users
```
![nestUsers](https://github.com/edierbra/Practicas_IoT_Edier/blob/main/Practica_3/Images/nestUsers.png?raw=true)

4. Para implementar el servicio de usuarios se modifica el archivo **users.service.ts** que esta dentro de la carpeta **users** de la siguiente manera. Este archivo contendra los usarios y sus respectivas contraseñas y se usara por el servicio de autenticacion.

```
import { Injectable } from '@nestjs/common';

export type User = {
   userId: number,
   username: string,
   password: string
};

@Injectable()
export class UsersService {
   private readonly users: User[] = [
      {
         userId: 1,
         username: 'edier',
         password: 'bravo',
      },
      {
         userId: 2,
         username: 'dario',
         password: 'bravo',
      },
      {
         userId: 2,
         username: 'edier',
         password: 'dario',
      },
   ];

   /**
      * Recupera los datos del usuario
      * @param username Nombre de usuario
      * @returns 
      */
   async findOne(username: string): Promise<User | undefined> {
      return this.users.find(user => user.username === username);
   }
}
```

5. Para que el servicio de usuarios este disponible en otros servicios es necesario configuran los modulos, en este caso se modifica el archivo **users.module.ts** de la siguiente manera.

```
import { Module } from '@nestjs/common';
import { UsersService } from './users.service';

@Module({
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}
```

En la carpera **usrs** se encuentra ptro archivo denominado **users.service.spec.ts**, el cual contiene las pruebas unitarias del servicio, el cual no se utilizara.

6. Para implementar un serviciuo de autenticacion (validar que el usuario y contraseña sean correctos), se modifica el archivo **auth.service.ts** de la siguiente manera.

```
import { Injectable } from '@nestjs/common';
import { UsersService } from '../users/users.service';

@Injectable()
export class AuthService {
constructor(private usersService: UsersService) {}

   async validateUser(username: string, pass: string): Promise<any> {
      const user = await this.usersService.findOne(username);
      if (user && user.password === pass) {
         const { password, ...result } = user;
         return result;
      }
      return null;
   }
}
```
7. Para la gestion de usuarios se modifica el archivo **auth.module.ts** de la siguiente manera.

```
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';
import { UsersModule } from '../users/users.module';

@Module({
   imports: [UsersModule], // Importa el módulo de usuarios
   providers: [AuthService]
})
export class AuthModule {}
```

8. Para validar a los usuarios se necesita crear un nuevo archivo denominado **local.strategy.ts** que se encuentra en la carpeta **auth** con en siguiente contenido.

```
import { Strategy } from 'passport-local';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable, UnauthorizedException } from '@nestjs/common';
import { AuthService } from './auth.service';

@Injectable()
export class LocalStrategy extends PassportStrategy(Strategy) {
   constructor(private authService: AuthService) {
      super();
   }

   async validate(username: string, password: string): Promise<any> {
      const user = await this.authService.validateUser(username, password);
      if (!user) {
         throw new UnauthorizedException();
      }
      return user;
   }
}
```

9. Para validar los usuarios con el anterior codigo, se modifica el archivo **auth.module.ts** de la siguiente forma.

```
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';
import { UsersModule } from '../users/users.module';
import { PassportModule } from '@nestjs/passport';
import { LocalStrategy } from './local.strategy';

@Module({
   imports: [UsersModule, PassportModule],
   providers: [AuthService, LocalStrategy]
})
export class AuthModule {}
```

10. una vez realizado los anteriores pasos se necesita aplicar estos cambios al controlador, por lo tanto el controlador queda de la siguiente forma.

```
import { Body, Controller, Delete, Get, Inject, Param, Patch, Post, Put, UseGuards } from '@nestjs/common';
import { TicketFullService } from '../../domain/services/ticketfull.service';

import { TicketFull } from '../../domain/models/ticketfull.model';
import { TicketFullController } from './ticketfull.controller';

import { AuthGuard } from '@nestjs/passport';

const errReturn = (e: Error, message: string) => {
  return {
    message: message,
    error: e
  }
}

@Controller()
export class TicketFullControllerImpl implements TicketFullController {
  constructor(@Inject('TicketFullService') private readonly tiketeService: TicketFullService) { }

  @Get()
  listTicketsFull() {
    try{
      return this.tiketeService.list();
    }
    catch(e){
      return errReturn(e, "Error al listar tiketes");
    }
  }

  @UseGuards(AuthGuard('local'))
  @Post()
  create(@Body() datos: TicketFull) {
    try{
      return this.tiketeService.create(datos);
    }
    catch(e){
      return errReturn(e, "Error al crear tikete");
    }
  }

  @Put(":id")
  update(@Body() datos: TicketFull, @Param('id') id: number) {
    try{
      return this.tiketeService.update(id, datos);
    }
    catch(e){
      return errReturn(e, "Error al modificar tikete");
    }
  }

  @Delete(":id")
  delete(@Param('id') id: number) {
    try{
      return this.tiketeService.delete(id);
    }
    catch(e){
      return errReturn(e, "Error al eliminar fecha de regreso del tikete");
    }
  }

  @Patch(":id/regreso/:regreso")
  updateReturn(@Param('id') id: number, @Param('regreso') regreso: Date) {
    try{
      return this.tiketeService.updateReturn(id, regreso);
    }
    catch(e){
      return errReturn(e, "Error al modificar fecha de regreso del tikete");
    }
  }
}
```

11. Se realiuzan las respectivas pruebas.

Al realisar una solicitud POST se observa lo siguiente.
![Post sencillo](https://github.com/edierbra/Practicas_IoT_Edier/blob/main/Practica_3/Images/postSencillo.png?raw=true)

En la anterior imagen se puede observar que la solicitud se realiza pero el usuario y contraseña quedan en el obejeto creado, lo cual no deberia pasar.

Si se quita el usuario y contraseña al hacer la solicitud POST se tiene un error 401 como se muestra a continuacion. Esto pasa por que se prohibe el servicio por falta de autenticacion de usuario.
 
![post sencillo fail](https://github.com/edierbra/Practicas_IoT_Edier/blob/main/Practica_3/Images/postSencilloFail.png?raw=true)

Hasta aqui la configuracion del proyecto queda como se muestra en este [Link.](https://github.com/edierbra/Practicas_IoT_Edier/tree/08eaf1e86cd77b2e191bb387cf38ea5acc13acfc)

# Autenticación con JWT

1. Para esto primero se instala el paquete **@nestjs/jwt**.

```
npm install --save @nestjs/jwt passport-jwt
npm install --save-dev @types/passport-jwt
```

![nestJwt](https://github.com/edierbra/Practicas_IoT_Edier/blob/main/Practica_3/Images/nestJWT.png?raw=true)

2. Se agrega el metodo login y algunas dependencias al archivo **auth.service.ts**

```
import { Injectable } from '@nestjs/common';
import { UsersService } from '../users/users.service';
import { JwtService } from '@nestjs/jwt';

@Injectable()
export class AuthService {
   constructor(
      private usersService: UsersService,
      private jwtService: JwtService
   ) {}

   async validateUser(username: string, pass: string): Promise<any> {
      const user = await this.usersService.findOne(username);
      if (user && user.password === pass) {
         const { password, ...result } = user;
         return result;
      }
      return null;
   }
   ````

3. Se implementa un endpoint para convertir las credenciales del usuario en un token JWT y de esta manera permirir el inicio de secion por parte de los usuarios. Para esto se crea un nuevo archivo denominado **auth.controller.ts** con el siguiente contenido.

```
import { Controller, Post, Request, UseGuards } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { AuthService } from './auth.service';

@Controller('auth')
export class AuthController {
   constructor(private authService: AuthService) {}

   @UseGuards(AuthGuard('local'))
   @Post('login')
   async login(@Request() req) {
      return this.authService.login(req.user);
   }
}
```

4. Para guardar la contraseña JWT se crea un archivo denominado **constants.ts** que tendar la siguiente constante.

```
export const jwtSecret = 'secretKey';
```

5. Para permitirle al passport encontarr el token al presentarse una peticion y la contraseña para permitirle validarlo, se crea un nuevo archivo denominado **jwt.strategy.ts** con el siguiente contenido.

```
import { ExtractJwt, Strategy } from 'passport-jwt';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable } from '@nestjs/common';
import { jwtSecret } from './constants';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
   constructor() {
      super({
         jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
         ignoreExpiration: false,
         secretOrKey: jwtSecret,
      });
   }

   async validate(payload: any) {
      return { userId: payload.sub, username: payload.username };
   }
}
```

6. Se configura el servicio JWT en el archivo **auth.module.ts**

```
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';
import { LocalStrategy } from './local.strategy';
import { UsersModule } from '../users/users.module';
import { PassportModule } from '@nestjs/passport';
import { JwtModule } from '@nestjs/jwt';
import { AuthController } from './auth.controller';

@Module({
   controllers: [AuthController],
   imports: [
      UsersModule,
      PassportModule,
      JwtModule.register({
         secret: "este es el secreto para generar JWT",
         signOptions: { expiresIn: '60m' },
      }),
   ],
   providers: [AuthService, LocalStrategy],
   exports: [AuthService],
   })
export class AuthModule {}
```

7. Para interceptar un token y validarlo en los endpoints donde se ha aplicado, se crera un nuevo archivo denominado **jwt-auth.guards.ts** con el siguiente contenido.

```
import { Injectable, ExecutionContext, UnauthorizedException } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {}
```

8. se actualizan los nuevos componentes en el archivo **auth.module.ts**

```
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';
import { LocalStrategy } from './local.strategy';
import { UsersModule } from '../users/users.module';
import { PassportModule } from '@nestjs/passport';
import { JwtModule } from '@nestjs/jwt';
import { AuthController } from './auth.controller';
import { JwtAuthGuard } from './jwt-auth.guard';
import { JwtStrategy } from './jwt.strategy';
import { jwtSecret } from './constants';

@Module({
   controllers: [AuthController],
   imports: [
      UsersModule,
      PassportModule,
      JwtModule.register({
         secret: jwtSecret,
         signOptions: { expiresIn: '60m' },
      }),
   ],
   providers: [AuthService, LocalStrategy, JwtStrategy, JwtAuthGuard],
   exports: [AuthService],
   })
export class AuthModule {}
```

10. Se llama desde la terminal de nuestra maqiona al endpoint que generara un token JWT.

```
curl -X POST http://localhost:3000/auth/login -d '{"username": "edier", "password": "bravo" }' -H "Content-Type: application/json"
```

![Token](https://github.com/edierbra/Practicas_IoT_Edier/blob/main/Practica_3/Images/accessToken.png?raw=true)

11. Se protegen los endpoints que sea necesario, para lo cual el controlador queda de la siguiente manera.

```
import { Body, Controller, Delete, Get, Inject, Param, Patch, Post, Put, UseGuards } from '@nestjs/common';
import { TicketFullService } from '../../domain/services/ticketfull.service';

import { TicketFull } from '../../domain/models/ticketfull.model';
import { TicketFullController } from './ticketfull.controller';

import { JwtAuthGuard } from 'src/auth/jwt-auth.guards';

const errReturn = (e: Error, message: string) => {
  return {
    message: message,
    error: e
  }
}

@Controller()
export class TicketFullControllerImpl implements TicketFullController {
  constructor(@Inject('TicketFullService') private readonly tiketeService: TicketFullService) { }

  @UseGuards(JwtAuthGuard)
  @Get()
  listTicketsFull() {
    try{
      return this.tiketeService.list();
    }
    catch(e){
      return errReturn(e, "Error al listar tiketes");
    }
  }

  @UseGuards(JwtAuthGuard)
  @Post()
  create(@Body() datos: TicketFull) {
    try{
      return this.tiketeService.create(datos);
    }
    catch(e){
      return errReturn(e, "Error al crear tikete");
    }
  }
  @UseGuards(JwtAuthGuard)
  @Put(":id")
  update(@Body() datos: TicketFull, @Param('id') id: number) {
    try{
      return this.tiketeService.update(id, datos);
    }
    catch(e){
      return errReturn(e, "Error al modificar tikete");
    }
  }
  @UseGuards(JwtAuthGuard)
  @Delete(":id")
  delete(@Param('id') id: number) {
    try{
      return this.tiketeService.delete(id);
    }
    catch(e){
      return errReturn(e, "Error al eliminar fecha de regreso del tikete");
    }
  }
  @UseGuards(JwtAuthGuard)
  @Patch(":id/regreso/:regreso")
  updateReturn(@Param('id') id: number, @Param('regreso') regreso: Date) {
    try{
      return this.tiketeService.updateReturn(id, regreso);
    }
    catch(e){
      return errReturn(e, "Error al modificar fecha de regreso del tikete");
    }
  }
}
```

finalmente la configuracion del proyecto queda como se muestra en este [Link.](https://github.com/edierbra/Practicas_IoT_Edier/tree/ff56f6154b51f3c797c53b372c1c9b25e94dd2f1)

12. Se realizan pruebas en la herramienta **Postman**.

En los endpoint protegidos con JWT es necesario usar un el token que anteriormente se solicito.

Para configurar el token en Postman, se elige la opcion de **Authorization**, en esta se escoge la opcion **Bearer Token** y se copea el token en la casilla ubicada a la derecha.

acontinuacion se muestran algunas imagenes al ejecutar ciertos metodos despues de implementar la autenticacion JWT.

**Metodo GET** (Consultar tiquetes )

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_3/Images/get.png?raw=true)

**Metodo POST** (Crear un tiquete)

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_3/Images/post.png?raw=true)

**Metodo PUT** (Actualiza un tiquete)

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_3/Images/put.png?raw=true)

**Metodo PATCH** (Modificar la fecha de regreso del tiquete)

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_3/Images/patch.png?raw=true)

**Metodo DELETE** (Eliminar un tiquete)

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_3/Images/delete.png?raw=true)




