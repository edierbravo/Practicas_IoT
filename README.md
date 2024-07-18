# **Práctica 2: Creando un servidor REST**

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

# DESARROLLO

## 1. Instalacion de herramientas

Se instala **gedit** el cual es un editor de textos.

```
sudo apt install gedit 
```

Se actualiza ubuntu mediante el comando.

```
sudo apt update
```

Se installa la herramienta **nodejs**.

```
sudo apt install nodejs -y
```

Se installa el gestor de paquetes **npm**.

```
sudo apt install npm
```

Se actualiza Nodejs.

```
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
```

Se validan las versiones de **nodejs** y **npm**.

```
node -v
npm -v
```

![Ver imagen](Images/versionNodeNpm.png)


Es importante crear un espacio para los recursos globales de nodejs para lo cual se usan los siguientes comandos.

```
cd
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo "export PATH=~/.npm-global/bin:$PATH" >> ~/.profile
source ~/.profile
```

No olvidar instalar **NestJS** usando los comandos.

```
npm i -g @nestjs/cli
source ~/.profile
```

## 2. Ejecutar Hello World

Para comprobar la correcta instalacion de las herramientas, se procede a ejecutar un pequeño proyecto.

Inicialmente se procede a crear la carpeta donde se alojara el proyecto.

```
cd ~/Documents
mkdir Servidores
cd Servidores
```

Se crea el proyecto usando el framework de **NestJS**, donde **practica2** es el nombre del proyecto.

```
nest new practica2
```

Durante la ejecucion del anterior comando elegir la opcion **npm**.

![Ver imagen](Images/nestNewPractica2.png)

Se identifica la direccion IP de nuestra maquina.

```
hostname -I
```

![Ver imagen](Images/HostmaneI.png)

Se ingresa a la carpeta **practica2**.

```
cd practica2
```

Dentro de esta carpeta se encuentra un archivo **package.json**, el cual define los scripts que se ejecutan con el comando **npm run** o **yarn run**. Este archivo contiene la siguiente informacion.

![Ver imagen](Images/catPackage.png)

Para ejecutar el ejemplo **Hello World** se usa el comando.

```
npm run start:dev
```

![Ver imagen](Images/npmRunStartDev.png)

De este modo ya se puede realizar peticiones GET desde la ruta raiz. Por defecto el servidor usa el puerto **3000**, para observar esto se ejecuta el siguiente comando.

```
netstat -tulpn | grep node
```

![Ver imagen](Images/grepNode.png)

Para hacer una peticion GET al servidor se usa.

```
curl http://localhost:3000
```

![Ver imagen](Images/HelloWorld.png)

Desde el navegador tambien se puede observar un comportamiento similar ingreando **http://192.168.20.194:3000**, donde la direccion IP es la de la maquina donde se esta trabajando.

![Ver imagen](Images/HelloWorlNavegador.png)

## 3. Publicar el codigo en Github

Inicialmente  se debe tener una cuenta en GitHub y haber creado un repositorio publico. 

Por facilidad se configura una llave SSH, para lo cual se sigue [este tutorial](https://medium.com/humantodev/configurar-ssh-github-enwindows-10-linux-y-macos-e843eb6d104e)

luego se ingresa a la carpeta donde se aloja nuestro proyecto, en este caso.

```
cd ~/Documents/Servidores/practica2
```

Se inicia nuestro repositorio en GitHub mediante el siguiente comando, donde la url es la url de nuestro repositorio.

```
git remote add origin https://github.com/edierbravo/Practicas_IoT.git
```

Se inicializa la carpeta de nuestro proyecto.

```
git init
```

Se añade un cambio del directorio de trabajo en el entorno de ensayo ejecutando el comando.

```
git add .
```

Se captura una instantánea de nuestro proyecto.

```
git commit -m "Primer commit"
```

Finalmente se carga el contennido de nuestro proyecto al repositorio Hithub.

```
git push --set-upstream origin master
```

![Ver imagen](Images/gitPushSetUpstreamOriginMaster.png)

## 4. Verbos HTTP

Para realizar esta parte de la practica se debe acceder a la maquina virtual mediante **VSCode**, para lo cual se debe tener en cuenta los siguientes pasos.

- Instalar el servicio **SSH** en nuestra maquina virtula, para lo cual se sigue [este tuturial](https://www.ionos.es/digitalguide/servidores/configuracion/ubuntu-ssh/)

- Instalar en VSCode la extencion **Remote-SSH**
- Acceder a la maquina virtual mediante **VSCode**, para lo cual se sigue [este tutorial](https://diarioprogramador.com/conectar-a-un-servidor-por-ssh-desde-visual-studio-code/)

Una vez haber ingresado a nuestra maquina virtual mediante **VSCode**, acceder a la carpeta de nuestro proyecto mediante el boton *Open Folder*.

Dentro de la carpeta de nuestro proyecto se encuentra un archivo llamado **app.controller.ts**, el cual ejecuta una peticion GET al servidor y tiene el siguiente contenido.

![Ver imagen](Images/appControllersTs.png)

Se modifica el contenido del metodo GET de este archivo de la siguiente manera y asi cambiar el comportamiento del servidor.

```
@Get()
getHello(): string {
  return "Hola Edier...";
}
```

Recordar que se debe guardar los cambios del archivo modificado y correr el servidor mediante el comando `npm run start:dev`

Para verificar los cambios realisados ejecutar en el terminal `curl http://192.168.20.194:3000` o `http://192.168.20.194:3000` en nuestro navegador.

Acontinuacion se presenta el resultado en el terminal de la maquina virtual.

![Ver imagen](Images/HolaEdier.png)

Para agregar un metodo POST se crea una variable para asi guardar un mensaje adicional y ser retomada por el metodo GET. Luego se agrega un metodo POST el cual tiene el parametro **nombre** como entrada, Por lo tanto **app.controller.ts** queda de la siguiente manera.

```
import { Controller, Get, Param, Post } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
   constructor(private readonly appService: AppService) {}

   private persona = "Edier...";

   @Get()
   getHello(): string {
      return `Hola: ${this.persona}`
   }

   @Post(':nombre')
   modificar(@Param('nombre') nombre: string): string {
      this.persona = nombre;
      return `Mensaje modificado: ${this.persona}`
   }
}
```
Para ejecutar el metodo POST en la terminal de la maquina virtual se usa.

```
curl -X POST http://<ip_del_servidor>:3000/Pikachu
```

![Ver imagen](Images/postTerminal.png)

Se sube los cambios frealizados, para lo cual se ejecutan los siguientes comandos

```
cd ~Documents/Servidores/practica_02
git add .
git commit -m "Se agrego un metodo POST"
git push origin master
```

![Ver imagen](Images/SeAgregoUnNuevoMetodoPOST.png)

## 5. Seleccionar un tema para moldear como una entidad.

El tema seleccionado es de reservas de vuelos, en el cual se tienen diferentes parametros como source (Lugar de origen), destination (Lugar de destino), date (Fecha), days (Tiempo entre ida y regreso al lugar de origen) y passengers (Cantidad de personas). Ademas se implementa el metodo GET, POST, PUT, PATCH (para modificar la cantidad de pasageros de una respectiva reserva de vuelos) y DELETE.

El contenido del archivo **app.controller.ts** queda de la siguiente manera.

```
import { Body, Controller, Delete, Get, Param, Patch, Post, Put } from '@nestjs/common';
import { AppService } from './app.service';

interface ticket {
  source: string,
  destination: string,
  date: string,
  days: number,
  passengers: number
}

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) { }

  private  tickets: ticket[] = [{
    source: "Popayan",
    destination: "Bogota",
    date: "16 Abril 2022",
    days: 6,
    passengers: 5
  }]

  @Get()
  getHello(): ticket[] {
    return this.tickets;
  }

  @Post()
  crear(@Body() datos: ticket): ticket {
    this.tickets.push(datos);
    return datos;
  }

  @Put(":id")
  modificar(@Body() datos: ticket, @Param('id') id: number): ticket | string {
    try{
    this.tickets[id] = datos
    return this.tickets[id];
    }
    catch{
      return `No fue posible modificar el vuelo en la posición ${id}`
    }
  }

  @Delete(":id")
  eliminar(@Param('id') id: number){
    try{
      this.tickets = this.tickets.filter((val, index) => index != id);
      return true;
    }
    catch{
      return false;
    }
  }

  @Patch(":id/passengers/:passengers")
  changePassengers(@Param('id') id: number, @Param('passengers') passengers: number): ticket | string{
    try{
      this.tickets[id].passengers = passengers;
      return this.tickets[id];
    }
    catch{
      return `No fue posible modificar el vuelo en la posición ${id}`
    }
  }
}
```

Para comprobar el correcto funcionamiento del servidor se usa la herramienta **Postman**.

**Metodo GET** (Consultar reservas de vuelos)

![Ver imagen](Images/get.png)

**Metodo POST** (Crear una nueva reserva)

![Ver imagen](Images/post.png)

**Metodo PUT** (Actualiza una reserva)

![Ver imagen](Images/put.png)

**Metodo PATCH** (Modificar cantidad de personas de una reserva)

![Ver imagen](Images/patch.png)

**Metodo DELETE** (Eliminar una reserva)

![Ver imagen](Images/delete.png)

Finalmente se actualiza el repositorio Hithub.

```
cd ~Documents/Servidores/practica_02
git add .
git commit -m "Se completó la tarea"
git push origin master
```
