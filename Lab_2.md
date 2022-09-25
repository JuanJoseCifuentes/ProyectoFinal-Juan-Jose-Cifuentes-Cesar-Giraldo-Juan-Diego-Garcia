  # Introducción
A este punto del curso, las redes de datos son un concepto con el que estamos bien familiarizados. Ya que entendemos el funcionamiento general de la mayor parte de los dispositivos de red, y tenemos una idea sólida de los procesos que hay detrás de las primeras capas de la comunicación de datos a través del internet, en este laboratorio buscamos afianzar esos conocimientos sumando una extra capa de complejidad al ejercicio anterior de montar una red de área local, puesto que ahora, además de expandir el tamaño de la misma, ubicándonos en una sala que permite que hasta 20 dispositivos se conecten a una red, vamos a hacer un esfuerzo consciente por aplicar cableado estructurado a la red.
No solo esto, en el espacio que antes nos habría servido para una sola red, se busca practicar la aplicación del software a la configuración de dispositivos de red, para poder tener múltiples de ellas. En concreto, se busca aplicar la configuración de VLAN, o redes de área local virtuales, sumado a la división física de redes mediante el cableado estructurado, para que nuestros propuestos usuarios puedan, desde el mismo espacio, acceder a una de 3 redes diferentes. De estas diferentes redes, la primera se trata de una red ajena a nuestro control, otra de configuración de dispositivos a través de cables de consola, y la última es nuestro interés principal en este laboratorio, que gracias a la configuración VLAN estará dividida a su vez en 4 subredes.
Así, en el presente informe nos disponemos a documentar todo el proceso de creación de la red final presentada en el documento adjunto. Explicaremos brevemente la disposición física de nuestra red, incluyendo nuestro cableado estructurado, las diferentes configuraciones necesarias para la creación de subredes VLAN, y algunos de los comandos y protocolos que se usaron para el correcto funcionamiento y verificación de la red, a fin de demostrar un conocimiento profundo de los temas y competencias propuestas hasta este punto del curso.

# Desarrollo de la solución

## Topología física en racks:

>![Terminal PC](/pics/imagenrack.png)

>
# Configuraciones
> **Configuración básica switch y creación de VLANs**
>
>Debemos primero entrar en la configuración del dispositivo, por lo que debemos entrar en modo de ejecución
>
>```
> Switch>enable
> Switch#configure terminal 
>```
>Una vez entramos en este modo, asignamos hostname, contraseña en la consola, además de contraseña en el caso de una conexión remota por Telnet.
>```
>Switch(config)#hostname Switch3
>Switch3(config)#line console 0
>Switch3(config-line)#password cisco
>Switch3(config-line)#login
>Switch3(config-line)#exit
>Switch3(config)#
>Switch3(config-line)#password cisco
>Switch3(config-line)#login
>Switch3(config-line)#exit
>```
>Ahora debemos asignar, la default gateway, el banner y hacer un shutdown a todas las interfaces del switch, incluidas las GigabitEthernet.
>```
>Switch3(config)#ip default-gateway 10.6.99.1
>Switch3(config)#banner motd #Buenos Dias Usuario#
>Switch3(config)#interface range fa0/1-24
>Switch3(config-if-range)#shutdown
>Switch3(config-if-range)#exit
>Switch3(config)#interface range Gi0/1-2
>Switch3(config-if-range)#shutdown
>Switch3(config-if-range)#exit
>```
>Asignamos enlaces "trunk" y los mandamos a la vlan nativa 99. También los dejamos activados.
>```
>Switch3(config)#interface range fa0/1-5
>Switch3(config-if-range)#switchport mode trunk
>Switch3(config-if-range)#switchport trunk native vlan 99
>Switch3(config-if-range)#no shutdown 
>Switch3(config-if-range)#exit
>```
>A continuación creamos las VLANs y las nombramos. 
>```
>Switch3(config)#vlan 30
>Switch3(config-vlan)#name Tecnologia
>Switch3(config-vlan)#vlan 10
>Switch3(config-vlan)#name Tesoreria
>Switch3(config-vlan)#vlan 15
>Switch3(config-vlan)# name Marketing
>Switch3(config-vlan)#vlan 99
>Switch3(config-vlan)#name vlan99
>Switch3(config-vlan)#exit
>```
>Asignamos las interfaces, a las respectivas VLANs basados en las indicaciones dadas por el profesor.
>```
>Switch3(config)#interface range fastEthernet0/6-9
>Switch3(config-if-range)#Switchport access vlan 30
>Switch3(config-if-range)#exit
>Switch3(config)#interface range fa0/10-12
>Switch3(config-if-range)#Switchport access vlan 15
>Switch3(config-if-range)#exit
>Switch3(config)#interface range fa0/13-24
>Switch3(config-if-range)#switchport access vlan 10
>Switch3(config-if-range)#exit
>Switch3(config)#interface range fa0/1-5
>Switch3(config-if-range)#switchport access vlan 99
>Switch3(config-if-range)#exit
>```
>Ahora asignamos una IP al switch, con la máscara de red, esto en la VLAN 99.
>```
>Switch3(config)#interface vlan 99
>Switch3(config-if)#ip address 10.6.99.4   255.255.255.0
>Switch3(config-if)#exit
>```
>Volvemos a activar las interfaces de todo el switch.
>```
>Switch3(config)#interface range fa0/6-24
>Switch3(config-if-range)#no shutdown
>Switch3(config-if-range)#exit
>Switch3(config)#interface range Gi0/1-2
>Switch3(config-if-range)#no shutdown
>Switch3(config-if-range)#exit
>```
>Ponemos la contraseña de el enable y guardamos la configuración.
>```
>Switch3(config)#enable password cisco
>Switch3(config)#exit
>Switch3#copy running-config startup-config 
>Destination filename [startup-config]? 
>Building configuration...
>[OK]
>```
>Este proceso se repite en cada switch.
# Verificaciones

>### Verificación de asignaciones IP a las interfaces NIC de los PC 
>
>Debemos de ir al command prompt en el desktop del PC deseado, para este caso verificaremos el PC 1, que en nuestro caso basados en la tabla 1 debe ser 10.5.30.7 usando el comando `Ipconfig` . 
Debemos fijarnos en la IPv4.1
>
>![Terminal PC](/pics/imagen1.png)
>
>Comprobaremos con el PC 6 donde el debe ser 10.5.10.
>
>![Terminal PC](/pics/imagen2.png)
>
> ### Verificación de la creación y configuración de las VLAN
>
>Para verificar que las vlans fueron creadas y están asignadas correctamente, nos dirigimos a cualquier switch. Ya que cada uno fue configurado individualmente de la misma forma. No usamos de Virtual Trunk Protocol (VTP) para configurar varios switches a la vez. 
El comando de cisco `show vlan brief` luego de ingresar la contraseña del switch nos dará la información requerida.
>
>![Terminal PC](/pics/imagen3.png)
>
> ### Verificación de conectividad entre PCs en la misma VLAN
>
>Para verificar la conectividad debemos hacer un `ping` al IP del PC en la misma VLAN, para este caso usamos la de marketing, por lo que usaremos los PC 2 y 5. Desde el PC 2 en la command prompt hacemos `ping 10.5.15.24`
>
>![Terminal PC](/pics/imagen4.png)
>
>Como podemos ver, el host nos devuelve una respuesta de que la misma cantidad de paquetes enviados, fueron recibidos por lo que no hubo pérdida y tenemos conexión con el PC 5. También nos arroja la latencia de la conexión.
>
> ### Verificación de conexión con la puerta de enlace 
>
>Para cada VLAN, en el router tenemos una puerta de enlace distinta (default gateway), por lo que podemos hacer la prueba dentro de esta, pero no es necesario ya que también podemos hacerlo a una diferente VLAN, a esto le llamamos comunicación intra-VLAN.
>
>![Terminal PC](/pics/imagen5.png)
>
> ### Verificación de conectividad entre dos PCs en VLANs distintas
>
>Realizamos `ping` a un computador fuera de la VLAN de mercadeo, en este caso el PC 1, el cual está en la VLAN de tecnología, tiene una IP de 10.5.30.7.
>
>![Terminal PC](/pics/imagen6.png)
>
>En este caso podemos ver que al hacer el ping por primera vez este manda un paquete broadcast para preguntar cual es la MAC de la IP destino, una vez la encuentra la IP destino va a enviar un mensaje unicast a IP host, en nuestro caso el paquete usado para hacer broadcast se pierde, pero una vez pasa esto todos los paquetes llegan, luego al volver a hacer ping al mismo host destino en otra VLAN, entra directo ya que tiene guardado la MAC destino. Una vez tenemos la conexión podemos decir que tenemos una comunicación intra-VLAN. 
>
> ### Verificación del protocolo STP 
>
>Con el comando `show spanning-tree` podemos ver que está configurado, nos mostrará por VLAN el spanning tree pero si detallamos son los mismos valores ya que se están usando las mismas interfaces del router para hacerlo. 
>
>![Terminal PC](/pics/imagen7.png)
>
>Al usar de este comando podemos darnos cuenta que si no especificamos una VLAN nos mostrara la de todas, en esta el root ID nos dice que este es el puente raíz de toda la red, por lo que la información del root ID debe ser la misma para los otros switches (puentes). 
Nos avisa que este es el puente raíz, y lo podemos comprobar fijándonos en que la dirección del `Bridge ID`, es la misma de la `Root ID` (cuadros verdes).
Por último, vemos que los puertos están todos con el rol de designados, esto se debe a que estos puertos son el acceso más cercano (o en este caso directo) al root. 
>último,
>![Terminal PC](/pics/imagen8.png)
>
>Ahora al trabajar en el switch 2, vemos que el Root ID es el mismo que el del Bridge ID del switch 1, corroborando que ese es el puente raíz, también nos dice por cual puerto hace la conexión. Y en la conexión nos dice que este cumple el rol de root. Ya que este nos lleva al puente raíz, o es el camino al puente raíz, ya que podría ser un caso donde tengamos más de 3 switches. 
Solo puede existir un puerto con rol de raíz, pero puede haber varios puertos designados, ya que estos pueden ser el camino a un puente raíz.
>
>![Terminal PC](/pics/imagen9.png)
>
>Por último en el switch 3 vemos que las interfaces de este nos muestra que la `Fa0/2` está siendo usada como designada, mientras que en el switch 2 no veíamos esta conexión, esto se debe a que esta es la conexión que está siendo bloqueada y al designar puertos busca la que tenga menor ID, a esta le asigna el designado, y a la otra lo bloquea por lo que no aparece.
>
>> **Designación del puente raíz:** 
En STP el puente raíz es designado con BPDU (Bridge Protocol Data Units) en este cada switch manda una señal diciendo que él es el puente raíz, pero al final el que tiene la prioridad más baja, es que se denomina puente raíz, pero en este caso todos tienen la misma por lo que nos fijamos en la MAC, la dirección, si comprobamos las anteriores imágenes la dirección del puente (marcada en recuadro verde) la de menor valor es la del switch 1. Esta es la razón por la que el switch 1 es el puente raíz.
>
>
> ## Verificación Telnet
>>
>>**PC a Router** 
>>>
>>Con ayuda del comando “telnet” seguido de la IP del dispositivo al que queremos hacer conexión remota podremos conectarnos directamente, como podemos observar en la imagen, nos mandó el banner que configuramos y nos da acceso al terminal del dispositivo.
>>
>>![Terminal PC](/pics/imagen10.png)
>
>>**PC a switch**
>>
>>Usamos el mismo comando, pero usamos las IP de los switches. En la imagen podemos ver cómo accedemos remotamente desde el PC5 a todos los switches sin problema.
>>
>>![Terminal PC](/pics/imagen11.png)

# Análisis de tráfico:
## Conectividad entre dos PCs en la misma VLAN:
>![Terminal PC](/pics/imagen_an_1.png)
>
>El paquete salió del PC1, llego al switch dos pasando por los nodos intermedios y luego desde ahi al switch 1, antes de pasar del switch 1 al Router, el paquete aun no sabia la dirección MAC a la que se dirigía.
>
>![Terminal PC](/pics/imagen_an_2.png)
>
>Una vez llega al Router, este lo dirige al switch 1, el cual es el puente raiz, de nuevo con la instrucción de que sea enviado al switch 3 y luego al PC4
>
>Al Salir del PC4 se evidencia que el paquete esta siendo devuelto al PC1 pues se indica la Dirección Mac Destino, la cual coincide con la del PC1
>
>![Terminal PC](/pics/imagen_an_3.png)
>
>Una vez regresa el paquete al PC1, se comprueba que existe conectividad entre el PC1 y el PC4. 
>
>Como resultado de este proceso, se hace innecesario volver a pasar por el router al enviar el siguiente paquete puesto que el PC1 ya conoce la dirección MAC del PC4. Así que el siguiente paquete solo pasa por los switches y llega directamente al PC4
>
## Conectividad con la puerta de enlace:
>![Terminal PC](/pics/imagen_an_4.png)
>
>Como se evidencia aqui, el paquete enviado por el PC2 va dirigido a la dirección IP del Default Gateway, pero aun se desconoce la dirección MAC.
>
>![Terminal PC](/pics/imagen_an_5.png)
>
>Una vez llega al puente raiz, este envia un mensaje broadcast a los dispositivos en la VLAN del PC2 para encontrar la dirección Mac de la IP del Gateway y obtiene una respuesta positiva del Router, por lo que envia un paquete de vuelta al PC2. Por otro lado, obtiene una respuesta negativa del PC5, por lo que el mensaje muere ahi.
>
>![Terminal PC](/pics/imagen_an_6.png)
>
>Tras conseguir la MAC del default gateway, el PC2 envía un mensaje de petición/respuesta directamente al Router, este lo devuelve y así se comprueba que existe una conexión exitosa.
>
## Conectividad entre dos PCs en VLANs distintas:
>![Terminal PC](/pics/imagen_an_7.png)
>
>Inicialmente, debido al protocolo ARP, se envia un mensaje broadcast por todas las vlans para encontrar la dirección MAC del dispositivo que se esta buscando y por esta razón se pierde el primer paquete, la primera vez que se intenta generar la conexión entro dos dispositivos en diferentes VLANs.
>
>![Terminal PC](/pics/imagen_an_8.png)
>
>La segunda vez que se hace, solo se envían mensajes de petición/respuesta debido a que ya se conoce la dirección MAC del otro dispositivo y por ende la ruta que se debe tomar para llegar a este. 

# Protocolos:

Obviamente, para la realización del laboratorio se utilizaron múltiples protocolos de red, sin embargo, muchos de ellos fueron explicados en el laboratorio anterior, y no consideramos necesario repetir sus definiciones, por lo que a continuación mencionaremos tan solo aquellos que son únicos del desarrollo de este laboratorio.

> **-STP:** A grandes rasgos, este protocolo tiene como propósito evitar las tormentas de multidifusión, momentos en los cuales un mensaje broadcast queda atrapado en un bucle dentro de la red, y genera tráfico de forma perpetua.
>Para conseguirlo, este protocolo se aplica a cualquier ciclo que pueda causar esto, en nuestro caso, se aplica al ciclo creado por los 3 switches, y consiste en escoger un switch principal, conocido como “Puente raíz”, que será el de menos ID de prioridad y MAC, y luego permitir que, a nivel lógico, se mantengan activos solo los puertos que forman parte de la ruta de menor costo desde cualquier otro switch al puente raíz. Finalmente, todos aquellos puertos que tienen una conexión que no es usada en ninguna ruta de coste menor son cerrados desde uno solo de los extremos, aquel que esté en el dispositivo de menor ID de prioridad y MAC.
>Los puertos que salen a la ruta de menor costo se llaman puertos raíz, los puertos a los que llega una conexión desde un puerto raíz se conocen como puertos designados, y aquellos bloqueados por el protocolo se conocen como puertos alternativos.
>
> **-VTP:** A pesar de no haber sido utilizado en la práctica merece una mención. Es un protocolo que permite que la información de configuración de VLAN se transmita a lo largo de todos los switches de la red, de forma que la versión más nueva se actualiza en todos los dispositivos sin necesidad de tener que configurarlos manualmente, configurando solo uno de ellos.
>__Se recomienda ser cuidadoso usando este protocolo, puesto que si, por ejemplo, se conecta un switch de una red externa, puede que la configuración previa se pierda por completo.__
>
> **-Tagging:** Este protocolo, estándar mundial ahora, puede funcionar también en switches y routers que no tengan capacidad VLAN. Sin embargo, en nuestro contexto este protocolo se encarga de añadir a las etiquetas que se aplican a las tramas de datos viajando por la red una sección especial que asocia de inmediato la información a la VLAN a la que debe transmitirse.
>No genera ningún problema con el entendimiento de la trama por parte del host receptor porque la tag con esta información de la VLAN se elimina en cuanto la trama es redireccionada a la subred indicada..
>
> **-Trunking (IEEE 802.1Q):** Este protocolo consiste en permitir a un switch o router transmitir la información perteneciente a múltiples VLAN usando una misma interfaz sirviéndose de las tags añadidas por el protocolo anteriormente mencionado.
>Gracias al trunking, por el mismo cable puede transmitirse información de todas las VLAN necesarias, y puesto que el router ahora puede redireccionar tramas usando una sola interfaz, permite que las VLAN de una red se comuniquen entre ellas sin necesidad de separar físicamente los cables que transmiten la información a cada VLAN.

# Desafíos afrontados:

Como viene siendo habitual, uno de los principales desafíos que tuvimos desarrollando el laboratorio fue la inconsistencia de Cisco Packet Tracer como entorno de trabajo. En más de una ocasión tuvimos que detener el progreso en el laboratorio para intentar encontrar un error que no estaba allí, y que solo se solucionaba después de cerrar y volver a abrir la aplicación. Por supuesto, esto complicó enormemente la búsqueda de errores, puesto que nunca estábamos seguros de si había un error nuestro o era tan solo un error del entorno de trabajo.

Otro pequeño detalle que vale la pena mencionar, fue la confusión que tuvimos con las ID’s de red de las diferentes VLAN’s. Para la configuración del router, a la hora definir las direcciones IP junto a sus máscaras de subred que debía tener cada VLAN, tardamos mucho en comprender que la ID de la red que se nos daba en el documento guía no era la IP deseada, y que la verdadera IP a aplicar debía corresponder a la primera dirección posible dada la ID de la red (a efectos prácticos, cambiando el último dígito por un 1).

Por último, y aún más desafiante, tuvimos que aprender lo básico de configurar los dispositivos de red que se nos pedían, por mucho que pudiéramos guiarnos con la lista de comandos proporcionada, y familiarizarnos con comandos de red que nunca habíamos usado hasta este punto para poder realizar las verificaciones necesarias para este documento.

# Conclusiones:

Este laboratorio nos ha servido en gran medida para poner en práctica los conocimientos teóricos vistos en este corte que aún no teníamos tan claros a nivel de aplicación en la realidad. Por supuesto, Cisco Packet Tracer es tan solo una herramienta de simulación, y no representa fielmente lo que es trabajar con estos dispositivos en la vida real, pero al menos nos permite poner forma desde 0 a conceptos hasta ahora puramente teóricos como lo eran las redes VLAN o el protocolo STP.

Además, el laboratorio nos ha acercado a la realidad en cuanto a cableado estructurado se refiere, en concreto, a algunas buenas prácticas que deberían tenerse en cuenta cuando se trabaja con dispositivos de red reales en entornos como el armario de conexiones, como lo es el montaje de patch panels de interconexión para comunicar dispositivos a lo largo de racks diferentes.

Otra habilidad que el laboratorio nos ha permitido desarrollar es la de detección y corrección de errores. Mediante las verificaciones propuestas por el documento guía pudimos verificar el buen funcionamiento de nuestra red paso por paso, y en caso de hallar algún error, fuimos forzados a servirnos de otros comandos de red y consola para encontrar que lo estaba causando, mejorando en definitiva nuestra capacidad de solucionar problemas en montajes de redes similares.

Por último, y aunque no las pusimos a prueba con aplicaciones reales, el laboratorio nos enseña la versatilidad de las redes locales, y cómo puede aprovecharse al máximo el espacio, bien sea adquiriendo y disponiendo nuevos dispositivos de red o aplicando capas de software que dejan diversificar sus funciones, tema especialmente interesante para nuestra carrera.



# Referencias:

-[1] J. Aranda, Cisco Switch - Basic Configuration. 2022.

-[2] Sunny Classroom. Spanning Tree Protocol (IEEE 802 1D). (1 de julio de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=Ilpmn-H8UgE

-[3]Sunny Classroom. How STP Elects Root Bridge with Hello BPDU? (7 de julio de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=BkGEwrzIK4g


