
# PRACTICA 2: Creando un servidor REST

## **Por:** *Edier Bravo*

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

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/versionNodeNpm.png?raw=true)


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

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/nestNewPractica2.png?raw=true)

Se identifica la direccion IP de nuestra maquina.

```
hostname -I
```

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/HostmaneI.png?raw=true)

Se ingresa a la carpeta **practica2**.

```
cd practica2
```

Dentro de esta carpeta se encuentra un archivo **package.json**, el cual define los scripts que se ejecutan con el comando **npm run** o **yarn run**. Este archivo contiene la siguiente informacion.

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/catPackage.png?raw=true)

Para ejecutar el ejemplo **Hello World** se usa el comando.

```
npm run start:dev
```

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/npmRunStartDev.png?raw=true)

De este modo ya se puede realizar peticiones GET desde la ruta raiz. Por defecto el servidor usa el puerto **3000**, para observar esto se ejecuta el siguiente comando.

```
netstat -tulpn | grep node
```

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/grepNode.png?raw=true)

Para hacer una peticion GET al servidor se usa.

```
curl http://localhost:3000
```

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/HelloWorld.png?raw=true)

Desde el navegador tambien se puede observar un comportamiento similar ingreando **http://192.168.20.194:3000**, donde la direccion IP es la de la maquina donde se esta trabajando.

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/HelloWorlNavegador.png?raw=true)

## 3. Publicar el codigo en Github

Inicialmente  se debe tener una cuenta en GitHub y haber creado un repositorio publico. 

Por facilidad se configura una llave SSH, para lo cual se sigue [este tutorial](https://medium.com/humantodev/configurar-ssh-github-enwindows-10-linux-y-macos-e843eb6d104e)

luego se ingresa a la carpeta donde se aloja nuestro proyecto, en este caso.

```
cd ~/Documents/Servidores/practica2
```

Se inicia nuestro repositorio en GitHub mediante el siguiente comando, donde la url es la url de nuestro repositorio.

```
git remote add origin https://github.com/edierbra/Practicas_IoT.git
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

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/gitPushSetUpstreamOriginMaster.png?raw=true)

## 4. Verbos HTTP

Para realizar esta parte de la practica se debe acceder a la maquina virtual mediante **VSCode**, para lo cual se debe tener en cuenta los siguientes pasos.

- Instalar el servicio **SSH** en nuestra maquina virtula, para lo cual se sigue [este tuturial](https://www.ionos.es/digitalguide/servidores/configuracion/ubuntu-ssh/)

- Instalar en VSCode la extencion **Remote-SSH**
- Acceder a la maquina virtual mediante **VSCode**, para lo cual se sigue [este tutorial](https://diarioprogramador.com/conectar-a-un-servidor-por-ssh-desde-visual-studio-code/)

Una vez haber ingresado a nuestra maquina virtual mediante **VSCode**, acceder a la carpeta de nuestro proyecto mediante el boton *Open Folder*.

Dentro de la carpeta de nuestro proyecto se encuentra un archivo llamado **app.controller.ts**, el cual ejecuta una peticion GET al servidor y tiene el siguiente contenido.

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/appControllersTs.png?raw=true)

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

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/HolaEdier.png?raw=true)

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

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/postTerminal.png?raw=true)

Se sube los cambios frealizados, para lo cual se ejecutan los siguientes comandos

```
cd ~Documents/Servidores/practica_02
git add .
git commit -m "Se agrego un metodo POST"
git push origin master
```

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/SeAgregoUnNuevoMetodoPOST.png?raw=true)

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

**Metodo GET**

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/get.png?raw=true)

**Metodo POST**

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/post.png?raw=true)

**Metodo PUT**

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/put.png?raw=true)

**Metodo PATCH**

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/patch.png?raw=true)

**Metodo DELETE**

![Ver imagen](https://github.com/edierbra/Practicas_IoT/blob/main/practica_2/Images/delete.png?raw=true)

Finalmente se actualiza el repositorio Hithub.

```
cd ~Documents/Servidores/practica_02
git add .
git commit -m "Se completó la tarea"
git push origin master
```
