# **Práctica 4: Desplegando en la nube**

## **Nombre:** Edier Dario Bravo Bravo

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

## Objetivos

1. Desplegar una aplicación en la nube.
2. Agregar una base de datos a la aplicación.

## Requisitos

1. Desarrollar Practica 3: Seguridad y Calidad

## Desarrollo

### 1. Desplegar una aplicacion en la nube.

Para el desarrollo de esta practica se utiliza el servicio de hosting gratuido **Deta**

Primero se descarga **Deta**

```
curl -fsSL https://get.deta.dev/cli.sh | sh
```

Se agrega la variable de entorno

```
echo "export PATH=~/.deta/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```

Se inicia sesion en Deta ejecutando

```
login deta
```

Si no tiene una cuenta deta debe crearla en este [Link](https://web.deta.sh/signup)

Luego se crea un punto de entrada de la aplicación, para lo cual se crea un archivo **index.ts** dentro de la carpeta **src** de nuestro proyecto. El archivo debe contener el siguiente contenido.

```
import { NestFactory } from '@nestjs/core';
import { ExpressAdapter } from '@nestjs/platform-express';
import { AppModule } from './app.module';

const createNestServer = async (expressInstance) => {
const app = await NestFactory.create(
   AppModule,
   new ExpressAdapter(expressInstance),
);

return app.init();
};

export default createNestServer;
```

Igualmente en la raiz de nuestro proyecto crear un archivo *index.js** con el siguiente contenido.

```
const express = require('express');
const createServer = require('./dist/index').default;

const app = express();
let nest;

app.use(async (req, res) => {
if (!nest) {
   nest = express();
   await createServer(nest);
}
return nest(req, res);
});

module.exports = app;
```

Se compila el proyecto mediante el siguiente comando. Este comando se debe ejecuitar en la carpeta raiz de nuestro proyecto.

```
nest build
```

Se publica la aplicacion para lo cual se ejecuta.

```
deta new --node ./<directorio_del_proyecto>/
```

- En nuestro caso: `deta new --node ./practica2/`
- NOTA: En **practica2** se encuentran los archivos correspondientes a la practica 3, la cual es la aplicacion que se va a desplegar a la nube en esta seccion. 

Una vez ejecutado el aterior comando te debe aparecer lo siguiente en la terminal

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/detaNewNode.png?raw=true)

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/detaNewNode2.png?raw=true)


Se desplega la aplicacion mediante el comando.

```
deta deploy <nombre_proyecto>
```

Se activan los logs de la aplicación, para lo cual en la raiz de nuestro proyecto se ejecuta el comando.

```
deta visor enable
```

Nos dirigimos a la pagina de **Deta** donde anteriormente se habia iniciado sesion, ahi nos ubicamos en la seccion de **micros** donde se observara una opcion con el nombre de nuestro proyecto en la cual se encontrara la **url** del servicio desplegado.

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/interfazDeta.png?raw=true)

Al abrir la url aparecera lo siguiente. lo cual corresponde a lo que se obtiene al implementar un metodo GET.

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/urlDeta.png?raw=true)

### 2. Conectado a una base de datos.

En este caso se empleara una base de datos no relacional orientada a documentos llamada MongoDB.

Para crear nuestra base de datos seguir los siguientes pasos:
1. Crea una cuenta gratuita en MongoDB Atlas, para lo cual se ingresa a este [LinK.](https://www.mongodb.com/atlas/database)

2. Presione el botón **Build a Cluster** y seleccione la opción **Free Tier**, se recomienda elegir **AWS** como proveedor de infraestructura y presione el botón **Create Cluster.**

3. Una vez creado el cluster, es necesario crear un usuario, En los campos correspondientes ingrese un nombre de usuario y una contraseña y Presione el botón **Add User.**

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/userCreate.png?raw=true)

4. Para facilitar la conexión la base de datos se recomienda crear una IP whitelist, para ello presione el botón **Add IP Address** y seleccione la opción **Allow Access from Anywhere**. Presione el botón **Confirm**. Si no encuentra esta opción ingrese una IP Address y en el campo asigne el valor **0.0.0.0**, finalmente presione el botón **Add Entry**.

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/ingresandoIp.png?raw=true)

5. Finalmente se guarda y despliegua haciendo click en el botón **Finish and close**. En este punto ya tendrá una base de datos MongoDB lista para ser utilizada.

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/dataBaseOk.png?raw=true)

Se instalan las dependencias de TypeORM y MongoDB 

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/installMongodb.png?raw=true)

En el panel de mongo atlas, en la sección **Connect** seleccione la opción **Connect your application**. Copie la url de conexión y reemplace el valor de la variable MONGO_URL en el archivo **~/Documents/Servidores/practica2/src/app.module.ts** por la url de conexion; ademas, se agrega la entidad en la configuración del módulo para habilitar el repositorio. Por lo tanto el archivo queda.

```
import { Module } from '@nestjs/common';
import { TicketFullControllerImpl } from './ticketfull/adapters/controladores/ticketfullImpl.controller';
import { TicketFullServiceImpl } from './ticketfull/domain/services/ticketfullImpl.service';
import { AuthModule } from './auth/auth.module';
import { UsersModule } from './users/users.module';
import { TypeOrmModule } from '@nestjs/typeorm';
import { TicketFullEntity } from './ticketfull/domain/entities/ticketfull.entity';

@Module({
  imports: [
    AuthModule,
    UsersModule,
    TypeOrmModule.forRoot({
      type: 'mongodb',
      url: 'mongodb+srv://Admin:Admin123db@cluster0.qnvonb3.mongodb.net/?retryWrites=true&w=majority',
      useNewUrlParser: true,
      useUnifiedTopology: true,
      synchronize: true, // Solo para desarrollo
      logging: true,
      autoLoadEntities: true,
    }),
    TypeOrmModule.forFeature([TicketFullEntity])
  ],
  controllers: [TicketFullControllerImpl],
  providers: [
    {
      provide: 'TicketFullService',
      useClass: TicketFullServiceImpl,
    }
  ],
})
export class AppModule {}
```

Se crea un nuevo archivo **ticketfull.entity.ts** en el directorio **src/ticketfull/domain/entities**, donde **ticketfull** es el nombre de nuestra entidad. El archivo queda.

```
import { Entity, Column, ObjectIdColumn } from 'typeorm';

@Entity('tikectfull')
export class TicketFullEntity {
   @ObjectIdColumn()
   id: string;

   @Column()
   passenger_name: string;

   @Column()
   source: string;

   @Column()
   destination: string;

   @Column()
   goingdate: Date;

   @Column()
   flight: string;

   @Column()
   returndate: Date;
}
```

Se agrega el constructor del servicio en el archivo **ticketfullImpl.service.ts** que se encuentra en la carpeta **src/ticketfull/domain/services**

```
...
@Injectable()
export class TicketFullServiceImpl implements TicketFullService {
  constructor(
    @InjectRepository(TicketFullEntity)
    private repository: MongoRepository<TicketFullEntity>,
  ) {}
...
```

Se reemplaza la implementación del modelo **TicketFull** por la entidad **TicketFullEntity** en el servicio **TicketFullServiceImpl**. De igual manera, como los datos toman tiempo para ser capturados del repositorio, se emplean Promesas. El cambio de **TicketFull** a **TicketFullEntity** se aplica en los servicios y controladores.

Se modifican en el archivo **ticketfull.service.ts** los metodos de tal manera que utilicen el repositorio. 

```
...
  public async list(): Promise<TicketFullEntity[]> {
    return await this.repository.find();
  }
 
  public async create(ticketfullData: TicketFullEntity): Promise<InsertResult> {
    const newTicketFull = await this.repository.insert(ticketfullData);
    return newTicketFull;
  }
 
  public async update(
    id: string,
    ticketfullData: TicketFullEntity,
  ): Promise<UpdateResult> {
    const updatedTicketFull = await this.repository.update(id, ticketfullData);
    return updatedTicketFull;
  }
 
  public async delete(id: string): Promise<boolean> {
    const deleteResult = await this.repository.delete(id);
    return deleteResult.affected > 0;
  }
 
  public async updateReturn(id: string, retorno: Date): Promise<UpdateResult> {
    const updatedTicketFull = await this.repository.update(id, { returndate: retorno });
    return updatedTicketFull;
  }
  ...
  ```

Finalmente se ejecuta el proyecto.

```
npm run start:dev
```

![](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/npmrunstartdev.png?raw=true)

Acontinuacion se hara la implementacion de los diferentes metodos mediante la herramienta **Postman**

**Metodo GET** (Consultar tiquetes )

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/get.png?raw=true)

**Metodo POST** (Crear un tiquete)

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/post.png?raw=true)

**Metodo PUT** (Actualiza un tiquete)

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/put.png?raw=true)

**Metodo PATCH** (Modificar la fecha de regreso del tiquete)

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/patch.png?raw=true)

**Metodo DELETE** (Eliminar un tiquete)

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_4/images/delete.png?raw=true)
