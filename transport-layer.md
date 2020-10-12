## Capa de transporte

# Introduccion
Proporciona comunicacion logica entre procesos de aplicacion en distintos hosts.

Este esta solo implemetado sobre los terminales
y no en los routers de la red.

TCP es un protocolo altamente regulado. Se 
encarga de mantener la fiabilidad de la red y el 
orden de los mensajes.

A su vez se responsabiliza de mantener el 
trafico razonable para no agotar la red.

Eso es distinto de UDP que no provee ningun 
mesanismo para garantizar que los segmentos 
lleguen y siempre intenta de utilizar toda la 
velocidad que le parezca necesaria durante el 
timepo necesario.

# Multiplexacion y demultiplexaciond e paquetes
En un ambiente de muchos programas se debe 
diferenciar mensajes de la capa de red para cada
uno de estos programas.

Un prorgrama tiene uno o mas sockets asignados. 
Estos tienen un identificador unico, dependiente
de la capa de transporte, sea esta UDP o TCP.

Este acto se llama demultiplexacion. Luego, 
reunir los fragmentos de datos en el host de 
origen desde los diferentes sockets, 
encapsulando cada fragmentode datos de 
informacion de cabecera para crear los segmentos 
a la capa de red es lo que se denomina 
multiplexacion.

Por esto vemos que cada segmento necesita un 
campo que identifique el socket de destino el 
cual debe ser unico. Este numero es de 16 bits y 
va del rango de 0 a 65535. Los puertos del 0 al 
1023 son puertos conocidos con aceso 
restringido, usados por HTTP en el puerto 80 y 
FTO en el puerto 21.

La linea de python
`client = socket(AF_INET, SOCK_DGRAM)`
crea un socket UDP con un numero de puerto 
automatico que no sea restringido y no este 
siendo utilizando por ningun otro puerto UDP. 

Utilizando `client.bind('', port_number)` podemos asignarle el numero de puerto deseado al socket.

Ahora un ejemplo para ver la multiplexacion y 
demultiplexacion.

Supongamos los host A y B. El host A crea un 
segmento de la capa de transporte con los datos 
de la aplicion como el numero de puerto de 
origen, el destino, y otros valores. Cuando el 
segmento llega al host B la capa de transporte 
revisa el numero de puerto para poder entregar 
el segmento a la aplicacion requerida. 

Un socket UDP queda completamente identificado 
por la tupla de ip, port_number. 

La historia en TCP es un poco distinta.

Las conexiones TCP quedan identificadas por 4 
valores, no 2. Por el IP de origen, IP de 
destino, puerto de origen y puerto de destino. 
Esta es la manera que se demulitplexan los 
segmentos TCP.

Un ejemplo de creacion de un socket TCP es el 
siguiente.

```python
client = socket(AF_INET, SOCK_STREAM)
client.connect((serverName, 12000))
```

Aqui se esta estableciendo una conexion al 
destino serverName en el puerto destino 12000. 

El proceso de servidor acepta la conexion de la 
siguiente manera:
```python
connection, address = server.accept()
```
Del lado servidor se toma nota de los cuatro 
valores identificadores de una conexion TCP.

Una vez establecida la conexion se podran 
realizar envios entre el cliente y servidor.

Aqui se ve que un host servidor puede dar 
soporte a muchos sockets TCP simultaneamente 
dada la manera de identificacion de estos. 
