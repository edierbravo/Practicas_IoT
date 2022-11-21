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
- Instalar las herramientas de red mediante el comando `sudo apt install net-tools`
- Instalar las herramientas **lsof** mediante el comando `sudo apt install lsof`

### Desarrollo
1. Se identifica la configuración de red por medio del comando:
```
ifconfig
```
[Ver imagen ifconfig](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_1/Images/ifconfig.png?raw=true)

2. Se Identifican los servicios y puertos ocupados en el sistema respectivamente con los comandos
```
ss | grep containerd
```
```
netstat | grep containerd
```
[Ver imagen de servicios y puertos ocupados](https://github.com/edierbra/Practicas_IoT/blob/main/Practica_1/servicios_puertos.png?raw=true)
