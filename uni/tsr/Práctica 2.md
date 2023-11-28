## 2.1.2. Patrón cliente/servidor `req/rep`

##### Un cliente y un servidor
- ¿Qué ocurre si pasamos un número de argumentos incorrecto? ¿y si están fuera de orden? 
> Si no ponemos los parámetros correctos, salta un **error previsto por el usuario**. Si están fuera de orden salta un **error no esperado** en la terminal.
-  Comprueba si el orden en que arrancamos los componentes afecta al resultado. Indica la razón
> No afecta puesto que los `req-rep` no se ven afectados por el orden de arranque.
- En relación con los mensajes multi-segmento:  
	- ¿De qué forma construye el emisor un mensaje multi-segmento? 
		> Lo construye mediante el uso de corchetes de la forma `[nombre, i]` 
	- ¿Cómo accede el receptor a los distintos segmentos del mensaje? 
		> El receptor accede de la misma manera en que se construyen los mensajes.
- El cliente finaliza tras recibir la respuesta a la cuarta petición. ¿Cuándo termina el servidor?
> El servidor NO termina nunca y se queda en espera.

##### Un cliente y dos servidores
-  Comprueba si el orden de arranque afecta al resultado. Indica la razón.
> No afecta ya que se trata de sockets `req/rep`.
- ¿Qué ocurre si ambos servidores reciben el mismo número de puerto?
> Salta un **error ya contemplado** diciendo que el puerto ya está en uso.
- ¿Qué ocurre si los dos servidores reciben un valor de segundos distinto (por ejemplo, 1 y 3 respectivamente)? ¿Afecta al orden en que se responde al cliente?
> Ocurre que el primer y el tercer mensaje son respondidos al segundo, y el segundo y el cuarto cada 3 segundos. No afecta al orden ya que se sigue una política *round-robin* para el reparto de mensajes.
- La práctica totalidad del tiempo lo consumen los servidores. ¿Conseguimos reducir a la mitad el tiempo de ejecución del cliente al utilizar 2 servidores?
> No se reduce nada el tiempo ya que el tiempo de respuesta de los servidores no varía. Para ver el tiempo que tardan se puede usar el comando siguiente (para medir `cliente1.js`): 
``` bash
time node cliente1.js localhost 9990 Pepe
```
- Si queremos usar 3 servidores, ¿hay que modificar el código del cliente?
> Sí ya que `cliente2.js` solo permite la conexión a 2 servidores de forma simultánea.
- Con un número par de peticiones, ¿podemos garantizar que cada servidor atiende la misma cantidad de peticiones?
> Sí ya que se sigue un reparto *round-robin* de respuesta por parte del cliente.

##### Dos clientes y un servidor
- Comprueba si el orden en que arrancamos los componentes afecta al resultado. Indica la razón.
> En este caso influye el orden en el que arrancamos puesto que si ejecutamos antes el cliente llamado Ana antes que Pepe pues acabará antes que Pepe. Esto se debe a que el primer mensaje de Ana entrará antes que los de Pepe en la cola de entrada del servidor.
- ¿Podemos asegurar que cada cliente recibe únicamente la respuesta a sus propias peticiones? Indica la razón.
> Sí ya que `req/rep` responde a cada `req` de forma atómica. Es decir, si el cliente Pepe envía un mensaje al servidor, se queda esperando **únicamente la respuesta a su mensaje**.
- En caso de plantear una cantidad distinta de clientes (por ejemplo, 3), ¿sería necesario modificar el código del cliente o del servidor?
> No ya que en el código no está limitado el número de conexiones permitidas
- En caso de que uno de los clientes termine antes de tiempo (ctrl-C), ¿el otro sigue recibiendo respuestas? Indica la razón.
> Sí que sigue recibiendo respuestas. 

## 2.1.3. Patrón *pipeline* `push/pull`

##### Origen1 --> Destino A-B
- Comprueba si el orden en que arrancamos los componentes afecta al resultado. Indica la razón.
> fkdjfkdnfks
- Indica la razón por la que el socket definido en origen.js no define socket.on('message',..)
> verovneover

##### Origen1 --> Filtro --> Destino A-B-C
- Comprueba si el orden en que arrancamos los componentes afecta al resultado. Indica la razón.
> rfergregeg
- Indica la razón por la que origen.js y destino.js definen un único socket cada uno, pero filtro.js define 2 sockets.
> rjejrvervre
- Si origen genera 4 mensajes y filtro retarda 2 segundos, ¿cuánto crees que tarda el último mensaje de origen en llegar a destino?
> egergerger

##### Origen2 --> [Filtro, Filtro] --> Destino A-(B,C)-D
- Comprueba si el orden en que arrancamos los componentes afecta al resultado. Indica la razón.
> lerlkgnerlkg
- ¿Cómo se reparte la entrega de mensajes a los filtros B y C?
> rlgjeogoerjg
- ¿Pueden trabajar B y C en paralelo (por ejemplo, si se ejecutasen en máquinas distintas)?
> lrgkjeogoerg
- ¿En qué orden llegan los mensajes a destino? ¿Cómo afectaría al comportamiento modificar los segundos del filtro C?
> lkjferjgkejerg


## 2.1.4. Patrón difusión `pub/sub`

- Indica si el orden en que arrancamos los componentes afecta al resultado.
> bdbdfbfdb
- ¿Qué pasa con los mensajes de Cultura?
> fbdbfdbdb
- ¿Puede recibir el mismo mensaje más de un suscriptor?
> bbdfbdfb
- ¿Cómo se puede cambiar la cantidad de mensajes que genera el publicador?
> dfgdgfdgdfg
- Los suscriptores no terminan. Piensa en una modificación para que terminen tras un tiempo determinado sin recibir mensajes.
> dfgbdbdfgfd
- Es posible que el publicador genere algún mensaje cuando todavía no ha procesado las conexiones de los suscriptores. ¿Qué pasa con esos mensajes?
> gdfgdfgdfg


## 2.2. Aplicación *chat*

- ¿En qué afecta el orden en que arrancamos los componentes?
> dfbdfbdfbd
- Indica la razón por la cual ambos puntos de conexión se crean en el servidor.
> fdgdfgfdgdf
- El servidor no mantiene una lista de clientes conectados. ¿Por qué razón?
> dgdfgfdgfdg
- Piensa cómo modificar un cliente para que atienda únicamente mensajes de algunos temas concretos.
> fgdfgfdgdfg


## 2.3. Publicador rotatorio

``` javascript
------------------------
```


## 2.4.1. Patrón `broker/workers`

- ¿Cómo afecta al resultado cambiar el orden de arranque de los workers (terminales 2 y 3)? ¿Y de los clientes (terminales 4, 5)?
> gegergregerg
- ¿Qué pasa si arrancamos el broker al final (el 1)? ¿Cómo afecta al sistema?
> ergegergreg


## 2.4.2. Estadísticas `broker`

- Modificación de `brokerRouterRouter.js`:
``` javascript
------------------
```

- Indica una estrategia para mantener en el broker estadísticas separadas para cada worker.
> rgergerg
- Indica cómo conseguir que se ejecute una acción de forma repetida (periódica).
> gergergreg
- Si llega una petición y se la pasamos al worker w, ¿debemos incrementar el número de peticiones atendidas por w (y el total) en ese momento, o cuando llegue la respuesta desde w?
> grergergreg


## 2.4.3. `Broker` para clientes + `Broker` para *workers*

- Modificación:
``` javascript
--------------------
```

- ¿Qué alternativas podemos usar para comunicar entre sí los dos brokers?
> ergergerg
- ¿Es conveniente avisar a broker1 cuando se da de alta un worker?
> rgegergerg
- ¿Es conveniente avisar a broker2 cuando llega una petición y no hay workers disponibles?
> ergergergerger


## 2.4.4. `Broker` tolerante a fallos de *workers*

- ¿Cuántas respuestas se obtienen? Indica qué trabajadores las han enviado.
> veveverve
- ¿Quedan clientes esperando?
> qpowopwie

Situados en el directorio `brokerToleranteFallos`:
- ¿Quedan clientes esperando?
> vfdfdvfdvfd
- ¿El cierre (caída) del worker es transparente para el cliente?
> dfdfvfdvdw2
- Únicamente se aborda posibles fallos de workers. Indica si se puede aplicar alguna estrategia ante un posible fallo del broker.
> dfvdfvdfvdfv

# Anexo: Guión de la práctica PDF

![[lab2-cas.pdf]]

#tsr