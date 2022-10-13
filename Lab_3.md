  # Introducción
A este punto del curso, las redes de datos son un concepto con el que estamos bien familiarizados. Ya que entendemos el funcionamiento general de la mayor parte de los dispositivos de red, y tenemos una idea sólida de los procesos que hay detrás de las primeras capas de la comunicación de datos a través del internet, en este laboratorio buscamos afianzar esos conocimientos sumando una extra capa de complejidad al ejercicio anterior de montar una red de área local, puesto que ahora, además de expandir el tamaño de la misma, ubicándonos en una sala que permite que hasta 20 dispositivos se conecten a una red, vamos a hacer un esfuerzo consciente por aplicar cableado estructurado a la red.
No solo esto, en el espacio que antes nos habría servido para una sola red, se busca practicar la aplicación del software a la configuración de dispositivos de red, para poder tener múltiples de ellas. En concreto, se busca aplicar la configuración de VLAN, o redes de área local virtuales, sumado a la división física de redes mediante el cableado estructurado, para que nuestros propuestos usuarios puedan, desde el mismo espacio, acceder a una de 3 redes diferentes. De estas diferentes redes, la primera se trata de una red ajena a nuestro control, otra de configuración de dispositivos a través de cables de consola, y la última es nuestro interés principal en este laboratorio, que gracias a la configuración VLAN estará dividida a su vez en 4 subredes.
Así, en el presente informe nos disponemos a documentar todo el proceso de creación de la red final presentada en el documento adjunto. Explicaremos brevemente la disposición física de nuestra red, incluyendo nuestro cableado estructurado, las diferentes configuraciones necesarias para la creación de subredes VLAN, y algunos de los comandos y protocolos que se usaron para el correcto funcionamiento y verificación de la red, a fin de demostrar un conocimiento profundo de los temas y competencias propuestas hasta este punto del curso.

# Desarrollo de la solución

A continuación, vamos a documentar todo el proceso que se llevó a cabo para el desarrollo del laboratorio, incluyendo la disposición de los dispostivos a nivel físico y lógico, el cálculo de subneteo que nos proporcionó las redes que utilizamos para cada VLAN, y la configuración de cada dispositivo, junto con las verificaciones que se nos pide en la guía de laboratorio.

## Topología física en racks:
>
>Utilizando el espacio de cableado estructurado realizado en el Workshop #4, conectamos los diferentes PCs a los switchs que les corresponden según lo pedido en la guía de laboratorio. El puerto fa0/1 no fue utilizado en ningún caso puesto que, dentro de la tabla de interfaces, no se le otorga ninguna VLAN, y por tanto no sirve para realizar conexiones troncales. Notese que, para cada switch, los puertos utilizados para la conexión de los PCs pertenecen siempre al mismo rango VLAN, aunque sean distintos para cada uno.
>
>![Terminal PC](/pics/imagenrack.png)

## Topología lógica:
>
>Se presenta una captura de la vista lógica del laboratorio. Notese que cada PCs se conecta en una VLAN diferente, a pesar de que esta vista no permite representar esto.
>
>![Terminal PC](/pics/imagenlogica.png)

## Subneteo:
>El interés principal de este laboratorio era la aplicación de subneteo para la creación manual de las VLAN requeridas. Para este propósito, se nos proporcionó el rango de IP 190.35.0.0/16 y se nos encomendó con crear nuestra propia propuesta para acomodar las necesidades de nuestro cliente, las cuales eran primero, tener como mínimo 5 redes, y segundo, permitir al menos 254 hosts en cada red.
>
>Para empezar, tuvimos que definir el criterio bajo el cual vamos a dividir el rango de IP. Al final, nos decidimos por usar el número de hosts como criterio, puesto que de haber utilizado el número de redes, habríamos tenido que dividir el rango en 8 redes, cada una con miles de hosts disponibles, y puesto que tan solo necesitamos 254 hosts por red, habría resultado en un uso muy ineficiente del rango original, además de limitar la posible expansión de la red en un futuro.
>
>Una vez hecho esto, representamos el número de hosts necesarios por red (254) como binario, y encontramos que nuestro identificador, es decir, el número de bits necesarios para esto, era 8. Posteriormente, representamos la máscara del rango (255.255.0.0) en binario, y reservando 8 ceros, debido a nuestro identificador, reemplazamos el resto de la máscara por unos, lo que resultó en una máscara de clase C (255.255.255.0). Por último, visto que el bit menos significativo cambiado era el 2^0 del tercer octeto, empezamos a sumar este valor a la red original, para encontrar los rangos que usaremos en este laboratorio. Estos rangos pueden verse en la siguiente captura.
>
>![Terminal PC](/pics/imagenredes.png)
>
>Habiendo encontrado los rangos, asignamos a cada uno una VLAN, junto con el número, interfaces y nombre que se especificaron en la guía de laboratorio. El resultado se ve a continuación:
>
>![Terminal PC](/pics/imagenvlan.png)
>
>Por último, nos dispusimos a construir la tabla de direcciones para cada dispositivo del sistema, de ahora en adelante nos referiremos a ella como Tabla 1. Tuvimos cuidado de asignar a cada computador una dirección y puerta de enlace perteneciente a su VLAN correspondiente, y a cada switch se le asignó una dirección de la VLAN 99. Para finalizar, nótese que las direcciones usadas en 0 no fueron utilizadas para nada, pues representan el ID de la red, y las direcciones terminadas en 1 fueron reservadas únicamente para las puertas de enlace, es decir, las subinterfaces del router 1.
>
>La Tabla 1. puede verse a continuación.
>
>![Terminal PC](/pics/tabla.png)
>

# Configuraciones
> **Configuración del Switch - Básica y Creación VLAN:**
>
>Debemos primero entrar en la configuración del dispositivo, por lo que debemos entrar en modo de ejecución
>
>```
> Switch>enable
> Switch#configure terminal 
>```
>Una vez entramos en este modo, asignamos hostname, contraseña en la consola, además de contraseña en el caso de una conexión remota por Telnet.
>```
>Switch(config)#hostname SW1
>SW1(config)#line console 0
>SW1(config-line)#password cisco
>SW1(config-line)#login
>SW1(config-line)#exit
>SW1(config)#
>SW1(config-line)#password cisco
>SW1(config-line)#login
>SW1(config-line)#exit
>```
>Ahora debemos asignar, la default gateway, el banner y hacer un shutdown a todas las interfaces del switch, incluidas las GigabitEthernet.
>```
>SW1(config)#ip default-gateway 190.35.0.1
>SW1(config)#banner motd #Se encuentra configurando el Switch 1. Ingrese la contrasena.#
>SW1(config)#interface range fa0/1-24
>SW1(config-if-range)#shutdown
>SW1(config-if-range)#exit
>SW1(config)#interface range Gi0/1-2
>SW1(config-if-range)#shutdown
>SW1(config-if-range)#exit
>```
>Asignamos enlaces "trunk" y los mandamos a la vlan nativa 99. También los dejamos activados.
>```
>SW1(config)#interface range fa0/2-6
>SW1(config-if-range)#switchport mode trunk
>SW1(config-if-range)#switchport trunk native vlan 99
>SW1(config-if-range)#no shutdown 
>SW1(config-if-range)#exit
>```
>A continuación creamos las VLANs y las nombramos. 
>```
>SW1(config)#vlan 35
>SW1(config-vlan)#name Biblioteca
>SW1(config-vlan)#vlan 20
>SW1(config-vlan)#name Estudiantes
>SW1(config-vlan)#vlan 40
>SW1(config-vlan)#name CuerpoDocente
>SW1(config-vlan)#vlan 55
>SW1(config-vlan)#name ServicioTecnico
>SW1(config-vlan)#vlan 99
>SW1(config-vlan)#name NATIVA
>SW1(config-vlan)#exit
>```
>Asignamos las interfaces, a las respectivas VLANs basados en las indicaciones de la tabla presentada en la guía de laboratorio.
>```
>SW1(config)#interface range fa0/7-10
>SW1(config-if-range)#Switchport access vlan 35
>SW1(config-if-range)#exit
>SW1(config)#interface range fa0/11-15
>SW1(config-if-range)#Switchport access vlan 20
>SW1(config-if-range)#exit
>SW1(config)#interface range fa0/16-20
>SW1(config-if-range)#switchport access vlan 40
>SW1(config-if-range)#exit
>SW1(config)#interface range fa0/21-24
>SW1(config-if-range)#switchport access vlan 55
>SW1(config-if-range)#exit
>SW1(config)#interface range fa0/2-6
>SW1(config-if-range)#switchport access vlan 99
>SW1(config-if-range)#exit
>```
>Ahora asignamos una IP al switch, con la máscara de red, esto en la VLAN 99.
>```
>SW1(config)#interface vlan 99
>SW1(config-if)#ip address 190.35.0.2   255.255.255.0
>SW1(config-if)#exit
>```
>Volvemos a activar las interfaces de todo el switch, menos la interfaz 1, que se ignora según la tabla de la guía de laboratorio.
>```
>SW1(config)#interface range fa0/7-24
>SW1(config-if-range)#no shutdown
>SW1(config-if-range)#exit
>SW1(config)#interface range Gi0/1-2
>SW1(config-if-range)#no shutdown
>SW1(config-if-range)#exit
>```
>Ponemos la contraseña del enable y guardamos la configuración.
>```
>SW1(config)#enable password cisco
>SW1(config)#exit
>SW1#copy running-config startup-config 
>Destination filename [startup-config]? 
>Building configuration...
>[OK]
>```
>Este proceso se repite en cada switch. El resultado final puede verse pasando el mouse sobre uno cualquiera. La ventana emergente, en la que podemos comprobar rápidamente la asignación de las VLAN, se ve así:
>
>![Terminal PC](/pics/vistaswitch.png)
>
>
> **Configuración del Router - Básica y Encapsulación:**
>
>Al igual que con el switch, la conexión básica consiste en asignar nombre, contraseñas de consola, telnet y enable y banner. 
>
>```
>Router>enable
>Router#configure terminal 
>Router(config)#hostname R1
>R1(config)#line console 0
>R1(config-line)#password cisco
>R1(config-line)#login
>R1(config-line)#exit
>R1(config)#
>R1(config-line)#password cisco
>R1(config-line)#login
>R1(config-line)#exit
>R1(config)#enable password cisco
>R1(config)#banner motd #Se encuentra configurando el Router. Ingrese la contrasena.#
>```
>Una vez hecho esto, encendemos la interfaz que estamos usando en el laboratorio (fa0/1).
>```
>R1(config)#interface fa0/1
>R1(config-if)#no shutdown
>R1(config-if)#exit
>```
>Ahora, se crean las subinterfaces (una por VLAN usada), se les asigna prtocolo de encapsulación y se configura su dirección IP.
>```
>R1(config)#interface fa0/1.35
>R1(config-if)#encapsulation dot1q 35
>R1(config-if)#ip address 190.35.1.1   255.255.255.0
>R1(config-if)#interface fa0/1.20
>R1(config-if)#encapsulation dot1q 20
>R1(config-if)#ip address 190.35.2.1   255.255.255.0
>R1(config-if)#interface fa0/1.40
>R1(config-if)#encapsulation dot1q 40
>R1(config-if)#ip address 190.35.3.1   255.255.255.0
>R1(config-if)#interface fa0/1.55
>R1(config-if)#encapsulation dot1q 55
>R1(config-if)#ip address 190.35.4.1   255.255.255.0
>R1(config-if)#interface fa0/1.99
>R1(config-if)#encapsulation dot1q 99 native
>R1(config-if)#ip address 190.35.0.1   255.255.255.0
>```
>Hecho esto, podemos guardar la configuración.
>```
>R1(config)#enable password cisco
>R1(config)#exit
>R1#copy running-config startup-config 
>Destination filename [startup-config]? 
>Building configuration...
>[OK]
>```
>Para comprobar rápidamente que la configuración haya sido exitosa, podemos pasar el mouse sobre el router unos segundos, y así ver sus subinterfaces y direcciones asignadas. Para este laboratorio, esta ventana se ve así:
>
>![Terminal PC](/pics/vistarouter.png)
>
>
> **Configuración del PC - Asignación de direcciones IP:**
> 
>Puesto que no tenemos un servidor DHCP en este laboratorio, todas las direcciones IPv4 de los PCs deben configurarse manualmente. A continuación, se puede ver como se configuró cada una, usando la dirección, máscara y puerta de enlace que le corresponde a cada PC según la tabla obtenida en el proceso de subneteo. El ejemplo mostrado corresponde a la configuración del PC6.
>
>![Terminal PC](/pics/vistapc.png)


# Verificaciones

>### Verificación de asignaciones IP a las interfaces NIC de los PC 
>
>Debemos de ir al command prompt en el desktop del PC deseado, para este caso verificaremos los PC 4, 3, 2 y 1 para poder ver las direcciones en las diferentes VLANs y redes, las direcciones usadas son las planteadas en el subneteo de la tabla 1 comando `Ipconfig` . 
Debemos fijarnos en la IPv4.
>
>![Terminal PC](/pics/imgIP_pc1.png)
>
>
>![Terminal PC](/pics/imgIP_pc2.png)
>
>
>![Terminal PC](/pics/imgIP_pc3.png)
>
>
>![Terminal PC](/pics/imgIP_pc4.png)
>
> ### Verificación de la creación y configuración de las VLAN 
>
>Para verificar qué las VLANs fueron creadas y están asignadas correctamente, nos dirigimos a cualquier switch. Ya que cada uno fue configurado individualmente de la misma forma. No usamos de Virtual Trunk Protocol (VTP) para configurar varios switches a la vez. 
El comando de cisco `show vlan brief` luego de ingresar la contraseña del switch nos dará la información requerida, podemos ver cómo las interfaces fueron distribuidas entre las VLANs.
>
>![Terminal PC](/pics/imgVLAN.png)
>
> ### Verificación de conectividad entre PCs en la misma VLAN
>
>Para verificar la conectividad debemos hacer un `ping` al IP del PC en la misma VLAN, para este caso usamos la de biblioteca, por lo que usaremos los PC  8 y 12. Desde el PC 8 en la command prompt hacemos `ping 190.35.1.1` (IP del PC 12)
>
>![Terminal PC](/pics/img_InterVLAN.png)
>
>Como podemos ver, el host nos devuelve una respuesta de que la misma cantidad de paquetes enviados, fueron recibidos por lo que no hubo pérdida y tenemos conexión con el PC 12. También nos arroja la latencia de la conexión.
>
> ### Verificación de conexión con la puerta de enlace 
>
>Para cada VLAN, en el router tenemos una puerta de enlace distinta (default gateway), por lo que podemos hacer la prueba dentro de esta, en este caso desde el PC 6 en la VLAN “Cuerpo docente/personal” nos conectaremos a su respectiva puerta de enlace 190.35.3.1 que se encuentra alojada en el Router.
>
>![Terminal PC](/pics/img_ptEnlace.png)
>
> ### Verificación de conectividad entre dos PCs en VLANs distintas
>
>Realizamos `ping` a un computador fuera de la VLAN, este proceso se le llama IntraVLAN , en este caso el PC 12, el cual está en la VLAN de biblioteca, y lo conectaremos con el PC 3, la cual esta en la VLAN Estudiantes y tiene una IP de 190.35.2.2.
>
>![Terminal PC](/pics/img_IntraVLAN.png)
>
>En este caso podemos ver que al hacer el ping por primera vez este manda un paquete broadcast para preguntar cual es la MAC de la IP destino, una vez la encuentra la IP destino va a enviar un mensaje unicast a IP host, en nuestro caso el paquete usado para hacer broadcast se pierde, pero una vez pasa esto todos los paquetes llegan, luego al volver a hacer ping al mismo host destino en otra VLAN, entra directo ya que tiene guardado la MAC destino.
>
> ## Verificación Telnet
>>
>>**PC a Router** 
>>>
>>Con ayuda del comando `telnet` seguido de la IP del dispositivo al que queremos hacer conexión remota podremos conectarnos directamente, como podemos observar en la imagen, nos mandó el banner que configuramos y nos da acceso al terminal del dispositivo.
>>
>>![Terminal PC](/pics/img_Router.png)
>
>>**PC a switch**
>>
>>Usamos el mismo comando, pero usamos las IP de los switches. En la imagen podemos ver cómo accedemos remotamente desde el PC5 a todos los switches sin problema.
>>
>>![Terminal PC](/pics/img_Switch.png)
>
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

En el desarrollo de este laboratorio, sorpresivamente la cantidad de errores o comportamientos inesperados disminuyó drasticamente, ya que entendemos mucho mas todo el entorno de Cisco Packet Tracer. 

El mayor desafío fue el subneteo, donde aplicamos los conocimientos vistos en clase, pero bajo unas condiciones muy distintas, ademas dándonos a nosotros la libertad de determinar cuál era la mejor opcion o en este caso cual era la mas optima. Ademas de que cómo quedaron las redes, ya no nos guiábamos por el número de la VLAN en la IP, si no por la determinada en el rango. Estas confusiones no significaron ningún problema, solo que sí alentaron todo el proceso.

Finalmente el proceso de configuración del router resulto confuso al principio ya que en anteriores laboratorios solo configuramos uno, por lo que no estabamos tan familiarizados, pero viendo nuestras notas del anterior laboratorio pudimos configurarlo sin percances.  


# Conclusiones:

Este laboratorio nos ha servido en gran medida para poner en práctica los conocimientos teóricos vistos en este corte que aún no teníamos tan claros a nivel de aplicación en la realidad. Por supuesto, Cisco Packet Tracer es tan solo una herramienta de simulación, y no representa fielmente lo que es trabajar con estos dispositivos en la vida real, pero al menos nos permite poner forma desde 0 a conceptos hasta ahora puramente teóricos como lo eran las redes VLAN o el protocolo STP.

Además, el laboratorio nos ha acercado a la realidad en cuanto a cableado estructurado se refiere, en concreto, a algunas buenas prácticas que deberían tenerse en cuenta cuando se trabaja con dispositivos de red reales en entornos como el armario de conexiones, como lo es el montaje de patch panels de interconexión para comunicar dispositivos a lo largo de racks diferentes.

Otra habilidad que el laboratorio nos ha permitido desarrollar es la de detección y corrección de errores. Mediante las verificaciones propuestas por el documento guía pudimos verificar el buen funcionamiento de nuestra red paso por paso, y en caso de hallar algún error, fuimos forzados a servirnos de otros comandos de red y consola para encontrar que lo estaba causando, mejorando en definitiva nuestra capacidad de solucionar problemas en montajes de redes similares.

Por último, y aunque no las pusimos a prueba con aplicaciones reales, el laboratorio nos enseña la versatilidad de las redes locales, y cómo puede aprovecharse al máximo el espacio, bien sea adquiriendo y disponiendo nuevos dispositivos de red o aplicando capas de software que dejan diversificar sus funciones, tema especialmente interesante para nuestra carrera.



# Referencias:

-[1] J. Aranda, Cisco Switch - Basic Configuration. 2022.

-[2] Sunny Classroom. Spanning Tree Protocol (IEEE 802 1D). (1 de julio de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=Ilpmn-H8UgE

-[3]Sunny Classroom. How STP Elects Root Bridge with Hello BPDU? (7 de julio de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=BkGEwrzIK4g


