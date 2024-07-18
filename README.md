# PRACTICA 1: PROTOCOLOS TCP Y UDP
**Por** *Edier Bravo*

## Objetivos de la práctica

1. Crear un entorno Linux virtual para evaluar servicios empleando herramientas como VirtualBox, Github y Docker.
2. Identificar en Linux mecanismos para la identificación de ocupación de puertos y a qué servicio está relacionado
3. Implementar un pequeño servicio cliente/servidor en Python empleando TCP y UDP, a partir de ello establecer cual es más adecuado para ciertos.

## 1. Configuracion del entorno.

En este caso se elige la distribucion lubuntu, y se realiza la correspondiente instalcion de la maquina virtual mediante la herramienta virtualbox.
Para la maquina se utilizo los siguiente recursos:
- 4Gb de RAM.
- 20Gb de almacenamiento.
- 2 procesadores.

## 2. Instalacion de docker.

Una vez uniciada la maquina virtual se procede a instalar docker, para lo cual se sigue la documentacion que encontramos en la pagina oficial de docker. <https://docs.docker.com/engine/install/ubuntu/>

Para probar la correcta instalacioon de docker se hace una pequeña prueba mediante el comando:
```
sudo docker run hello-world
```
![Ver imagen: Hello World](Images/hello_world.png)

## 3. Desarrollo
### Prerequisitos
- Tener la configuracion de red de la maquina virtual en **adaptador puente**.
- Instalar las herramientas de red  `sudo apt install net-tools`.
- Instalar las herramientas **lsof**  `sudo apt-get install lsof`.
- (Opcional) Instalar el gestor de archivos **gedit** `sudo apt-get install gedit`.

### 3.1. Identificar la configuración de red
Para esto se usan los siguientes comandos.

```
ifconfig
```
![Ver imagen: ifconfig](Images/ifconfig.png)

### 3.2. Identificar los servicios y puertos ocupados
Para esto se usan los siguientes comandos.
```
ss | grep containerd
netstat | grep containerd
lsof | grep containerd
```
![Ver imagen: de servicios y puertos ocupados](Images/servicios_puertos.png)

### 3.3. Conexion TCP (Cliente y Servidor en la misma maquina Lubuntu)

Se crea un archivo denominado **server.py**, en este caso el archivo se guarda en el directorio **/Documentos/server**.
```
cd Documentos/
mkdir serverTCP
cd serverTCP/
touch server.py
```
Dentro del archivo **server.py** de escribe el siguiente codigo.

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
Dentro del directorio **/Documentos/server** se crea un nuevo archivo denominado **client.py**, en el cual se tiene el siguiente codigo.

```
import socket
import sys

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server_address = ('0.0.0.0', 10000)
print("Iniciando cliente...")
sock.connect(server_address)

try:
    message = b'Este es un mensaje'
    print("Enviando {!r}".format(message))
    sock.sendall(message)
    
    amount_rcv = 0
    amount_exp = len(message)
    
    while amount_rcv < amount_exp:
        data = sock.recv(16)
        amount_rcv += len(data)
        print("Recibiendo {!r}".format(data))
finally:
   print("Cerrando socket")
   sock.close()
```

Dentro del directorio **/Documentos/server** se ejecuta el archivo **server.py** `python3 server.py` y luego el archivo **client.py** `python3 client.py`.

![Ver imagen: conexion TCP](Images/tcp.png)

Al ejecutar el comando `lsof -i -P -n` se puede observar que el puerto **10000** que se utilizo en la conexion servidor-cliente esta usado.

![Ver imagen: puerto 10000 ocupado](Images/puerto_10000.png)

### 3.4. Conexion UDP (Cliente y Servidor en la misma maquina Lubuntu).

Se crea un archivo denominado **server.py**, en este caso el archivo se guarda en el directorio **/Documentos/serverUDP**. Este archivo tendra el siguiente contenido.

```
import socket
import sys

# Create a UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Bind the socket to the port
server_address = ('localhost', 10000)
print('starting up on {} port {}'.format(*server_address))
sock.bind(server_address)

while True:
    print('\nwaiting to receive message')
    data, address = sock.recvfrom(4096)

    print('received {} bytes from {}'.format(
        len(data), address))
    print(data)

    if data:
        sent = sock.sendto(data, address)
        print('sent {} bytes back to {}'.format(
            sent, address))
```
Dentro del directorio **/Documentos/serverUDP** se crea un nuevo archivo denominado **client.py**, en el cual se tiene el siguiente codigo.

```
import socket
import sys

# Create a UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

server_address = ('localhost', 10000)
message = b'This is the message.  It will be repeated.'

try:

    # Send data
    print('sending {!r}'.format(message))
    sent = sock.sendto(message, server_address)

    # Receive response
    print('waiting to receive')
    data, server = sock.recvfrom(4096)
    print('received {!r}'.format(data))

finally:
    print('closing socket')
    sock.close()
```

Dentro del directorio **/Documentos/serverUDP** se ejecuta el archivo **server.py** `python3 server.py` y luego el archivo **client.py** `python3 client.py`.

![Ver imagen: conexion UDP](Images/udp.png)

Al ejecutar el comando `lsof -i -P -n` se puede observar que el puerto **10000** que se utilizo en la conexion servidor-cliente esta usado.


![Ver imagen: puerto 10000 ocupado](Images/puerto_10000UDP.png)

### 3.5. Conexion TCP (Cliente Windows y servidor Lubuntu).

En este caso el servidor esta en la maquina Lubuntu, por lo tanto  se utiliza el mismo archivo **server.py** del caso de TCP. Por otro lado para el cliente se crea un archivo **cliente.py** en la maquina nativa Windows, en el cual se tiene el siguiente codigo.

```
import socket
import sys

# Create a TCP/IP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect the socket to the port where the server is listening
server_address = ('192.168.20.194', 10000)
print('connecting to {} port {}'.format(*server_address))
sock.connect(server_address)

try:

    # Send data
    message = b'Conectandome desde Windows Client'
    print('sending {!r}'.format(message))
    sock.sendall(message)

    # Look for the response
    amount_received = 0
    amount_expected = len(message)

    while amount_received < amount_expected:
        data = sock.recv(16)
        amount_received += len(data)
        print('received {!r}'.format(data))

finally:
    print('closing socket')
    sock.close()
```

Se ejecuta el archivo **server.py** `python3 server.py` en la maquina Lubuntu y luego el archivo **cliente.py** en Windows.

![Ver imagen: conexion TCP desde diferentes maquinas](Images/tcpWindows.png)

### 3.6. Conexion UDP (Cliente Windows y servidor Lubuntu).

En este caso el servidor esta en la maquina Lubuntu, por lo tanto  se utiliza el mismo archivo **server.py** del caso de UDP. Por otro lado para el cliente se crea un archivo **cliente.py** en la maquina nativa Windows, en el cual se tiene el siguiente codigo.

```
import socket
import sys

# Create a TCP/IP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect the socket to the port where the server is listening
server_address = ('192.168.20.194', 10000)
print('connecting to {} port {}'.format(*server_address))
sock.connect(server_address)

try:

    # Send data
    message = b'Conectandome desde Windows Client'
    print('sending {!r}'.format(message))
    sock.sendall(message)

    # Look for the response
    amount_received = 0
    amount_expected = len(message)

    while amount_received < amount_expected:
        data = sock.recv(16)
        amount_received += len(data)
        print('received {!r}'.format(data))

finally:
    print('closing socket')
    sock.close()
```

Se ejecuta el archivo **server.py** `python3 server.py` en la maquina Lubuntu y luego el archivo **client.py** en Windows.

![Ver imagen: conexion TCP desde diferentes maquinas](Images/udpWindows.png)

De acuerdo a lo anterior se puede decir que entre sus cararcteristicas principales se encuentran:

| **Caracteristica**    | **TCP**         | **UDP**           |
|-------------------|-------------|---------------|
| **Servicio**  | Es un protocolo orientado a la conexión; es decir, los dispositivos deben establecer una conexión antes de transmitir datos.   | Es un protocolo orientado a datagramas. |
| **Velocidad**   | Mas lento que UDP | Mas rapido que TCP  |
| **Fiabilidad** | Es confiable ya que garantiza la entrega de datos al  destino. | No se puede garantizar la entrega de datos al destino.  |
| **Retransmisión** | La retransmisión de paquetes perdidos es posible.  | No hay retransmisión de paquetes perdidos.  |
|  **Comprobación de errores** | Proporciona control de flujo y reconocimiento de datos.  | Solo tiene el mecanismo básico de verificación de errores mediante sumas de verificación.  |
