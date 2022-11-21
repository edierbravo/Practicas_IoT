# PRACTICA 1
**Por** *Edier Bravo*

## 1. Configuracion del entorno

En este caso se elige la distribucion lubuntu, y se realiza la correspondiente instalcion de la maquina virtual mediante la herramienta virtualbox.
Para la maquina se utilizo los siguiente recursos:
- 4Gb de RAM
- 20Gb de almacenamiento
- 2 procesadores

## 2. Instalacion de docker

Una vez uniciada la maquina virtual se procede a instalar docker, para lo cual se sigue la documentacion que encontramos en la pagina oficial de docker. <https://docs.docker.com/engine/install/ubuntu/>

Para probar la correcta instalacioon de docker se hace una pequeña prueba mediante el comando:
```
sudo docker run hello-world
```
[Ver imagen Hello World](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_1/Images/servicios-puertos.png?raw=true)

## 3. Reconocimiento de herramientas de red
### Prerequisitos
- Tener la configuracion de red de la maquina virtual en **adaptador puente**.
- Instalar las herramientas de red  `sudo apt install net-tools`
- Instalar las herramientas **lsof**  `sudo apt-get install lsof`
- (Opcional) Instalar el gestor de archivos **gedit** `sudo apt-get install gedit`

### Desarrollo
1. Se identifica la configuración de red por medio del comando:
```
ifconfig
```
[Ver imagen ifconfig](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_1/Images/ifconfig.png?raw=true)

2. Se Identifican los servicios y puertos ocupados en el sistema mediante los comandos
```
ss | grep containerd
netstat | grep containerd
lsof | grep containerd
```
[Ver imagen de servicios y puertos ocupados](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_1/Images/servicios_puertos.png?raw=true)

3. Evaluar scripts en Python

Se crea un archivo denominado **server.py**, en este caso se guarda el archivo en el directorio **/Documentos/server**
```
cd Documentos/
mkdir server
cd server/
touch server.py
```
Dentro de este archivo de escribe el siguiente codigo
```
import socket
import sys

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server_address = ('0.0.0.0', 10000)
print("Iniciando servidor...")
sock.bind(server_address)

sock.listen(1)

while True:
    print("Esperando por una conexion")
    connection, client_address = sock.accept()
    try:
        print("Conectando desde: ", client_address)
        while True:
            data = connection.recv(16)
            print('Recibido {!r}'.format(data))
            if data:
                print("Enviando datos de regreso")
                connection.sendall(data)
            else:
                print("No hay datos desde el cliente", client_address)
                break
    finally:
        connection.close()
```
