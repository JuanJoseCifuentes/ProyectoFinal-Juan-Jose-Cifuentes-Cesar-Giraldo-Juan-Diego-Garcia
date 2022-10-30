# Introducción
En el presente documento se encuentra detallada la planeación, desarrollo y análisis pertinente del proyecto final que se desarrolló para la materia de Redes y Comunicación de Datos.

Como se verá detalladamente más adelante, las diferentes partes de este proyecto buscan poner a prueba los conocimientos obtenidos a lo largo del curso, además de delegarnos una labor investigativa considerable para poner en práctica los conceptos puramente teóricos que se presentaron durante los meses anteriores.

Así, el objetivo de este proyecto fue el de construir una topología compleja, descrita por una red de área extensa (WAN) compuesta a su vez de múltiples redes de área local (LAN). Dentro de cada una de estas redes, se buscó poner a prueba nuestra capacidad de implementar los varios protocolos, formas de conexión y servicios de red que se discutieron a lo largo del curso. Desde poner en práctica nuestro dominio sobre VLANs y DHCP en la red LAN de Campus, hasta investigar sobre la implementación de una WLAN mediante puntos de acceso livianos en la red LAN de Oficinas, pasando por la planeación de direccionamiento de cada una de las LANs y WAN usando Subneteo, este proyecto es la culminación de todas las habilidades que obtuvimos en el curso.

En la primera parte, se describirá a detalle los procesos tomados para plantear el subneteo de red, configurar los diversos dispositivos de red que se encuentran en la topología, e implementación de los diferentes servicios y protocolos de red solicitados. A continuación, documentaremos las verificaciones del funcionamiento de cada uno de los puntos requeridos en la guía del proyecto. Por último, en la sección de análisis esperamos demostrar nuestro dominio del tema más allá de la implementación, monstrando entendimiento del trayecto que toma la información cuando es transmitida por una red de comunicación de datos, acompañando las coclusiones obtenidas del proyecto con la descripción de los problemas que afrontamos.

# Desarrollo de la solución

## Topología lógica:
>
>En este apartado, presentamos una captura de la topología lógica montada para el proyecto. En la primera imagen, se puede ver descrita la red WAN en su totalidad, con las diferentes redes LAN diferenciadas por rectángulos negros. En las imágenes subsiguientes se puede ver más detalladamente cada red LAN.
>
>### **Topología Completa**
>![Terminal PC](/pics/topologia_logica.png)
>
>### **Topología de cada LAN**
>
>**LAN Campus**
>![Terminal PC](/pics/topologia_logica_vlan.jpeg)
>
>**LAN Oficinas**
>![Terminal PC](/pics/topologia_logica_oficinas.jpeg)
>
>**LAN Mi Casa Inteligente**
>![Terminal PC](/pics/topologia_logica_casa.jpeg)
>
>**LAN Servidores**
>![Terminal PC](/pics/topologia_logica_servidores.jpeg)


## Topología física en racks:
>
>Para la topología física, se separó cada LAN en edificios diferentes, sobre los que se dispusieron los dispositivos con cierto cuidado para mantener la disposición que tendrían en una situación real.
>
>Las siguientes capturas ilustran lo dicho anteriormente, detallando la disposición de cada LAN. Nótese que para la red LAN de Campus se utilizó el cuarto de cableado estructurado presentado en el Workshop #4.
>
>### **Topología Completa**
>![Terminal PC](/pics/topologia_fisica.png)
>
>### **Topología de cada LAN**
>
>**LAN Campus**\
>Sala de computadores
>![Terminal PC](/pics/topologia_fisica_vlan1.png)
>Sala de cabeado
>![Terminal PC](/pics/topologia_fisica_vlan2.png)
>
>**LAN Oficinas**
>![Terminal PC](/pics/topologia_fisica_oficinas.png)
>
>**LAN Mi Casa Inteligente**
>![Terminal PC](/pics/topologia_fisica_casa.png)
>
>**LAN Servidores**
>![Terminal PC](/pics/topologia_fisica_servidores.png)


## Subneteo:
>
>Para esta documentación, no explicaremos a detalle el proceso de subneteo, puesto que no es el principal foco de atención del proyecto. Nos centraremos en explicar brevemente los criterios bajo los que decidimos realizar el proceso de subneteo en cada uno de los 3 casos diferentes que nos presenta la guía de proyecto, junto a una captura del proceso entero en caso de que sea del interes del lector.
>
>En caso de que se desee leer una descripción exacta del proceso de realizar cada subneteo, le pedimos al lector que acceda a la documentación del laboratorio #3, en la que está explicación se hace a detalle. El link para acceder al documento se encuentra a continuación: https://github.com/JuanJoseCifuentes/Lab-3-Juan-Jose-Cifuentes-Cesar-Giraldo-Juan-Diego-Garcia/blob/master/Lab_3.md
>
>**Subneteo red Campus:**
>
>Para esta red, se tuvieron en cuenta dos criterios. Primero, cada una de las subredes debía tener como mínimo 254 hosts, y segundo, dado que se necesitaba una red por VLAN, el resultado debía tener mínimo 5 redes. Dado que optar por cumplir solo el criterio de las redes habría resultado en un desperdicio enorme de direcciones que podrían ser usadas para expandir la red, se optó por seguir el primer criterio.
>
>El proceso subisguiente puede verse a continuación
>
>![Terminal PC](/pics/subneteo_campus.png)
>
>**Subneteo red Servidores:**
>
>Para el subneteo de esta red, tan solo se tuvo en cuenta el criterio de que cada red debía tener, mínimo, 30 hosts. Dado que el subneteo solo se aplica para hallar una red que usar en los dos servidores junto a la interfaz de router correspondiente, el número de redes no se consideró.
>
>El proceso subisguiente puede verse a continuación
>
>![Terminal PC](/pics/subneteo_servidores.png)
>
>**Subneteo red WAN:**
>
>En el desarrollo de este subneteo se optó por dividir el rango en mínimo 2 redes diferentes. Esto debido a que, puesto que el router ISP ya tiene ocupados todas sus interfaces, no hay mucho espacio para expandir la red WAN, por lo que el desperidicio de redes no es un problema tan notorio.
>
>El proceso subisguiente puede verse a continuación
>
>![Terminal PC](/pics/subneteo_wan.png)
>


## Tablas de enrutamiento
>
>Después de montar la topología y realizar el subneteo de las diferentes redes, lo último que hacer en la etapa de planeación es crear las tablas de enrutamiento, en las que consignar fácilmente las direcciones de todos los dispositivos de red que han de ser configurados a continuación.
>
>Para esto se tomaron en cuenta no solo las direcciones que se obtuvieron del proceso de subneteo anterior, sino las direcciones dadas en el documento guía del proyecto. Nótese que para este punto ya se habían transformado todas las direcciones para ajustarse a nuestro número de grupo.
>
>A continuación se presentan todas las tablas preparadas:
>
>**LAN Campus**
>![Terminal PC](/pics/tabla_campus.png)
>
>**LAN Servidores**
>![Terminal PC](/pics/tabla_servidores.png)
>
>**LAN Oficina**
>![Terminal PC](/pics/tabla_oficina.png)
>
>**LAN Mi Casa Inteligente**
>![Terminal PC](/pics/tabla_casa.png)
>
>**WAN**
>![Terminal PC](/pics/tabla_wan.png)
>


# Configuraciones
> **Configuración básica de los dispositivos de red:**
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
>SW1(config)#banner motd #Se encuentra configurando el Switch 1. Ingrese la contrasena.#
>SW1(config)#enable password cisco
>```
> ## **Campus**
> **Configuración de los switches:**
>
>Después de hacer la configuración básica de los switches, el primer paso es definir para cada uno una dirección de la default gateway, es decir, del router 1, y apagar sus interfaces.
>```
>SW1(config)#ip default-gateway 191.55.0.1
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
>Ahora asignamos una IP al switch junto con la máscara de red. Esto se hace en la VLAN 99.
>```
>SW1(config)#interface vlan 99
>SW1(config-if)#ip address 191.55.0.2   255.255.255.0
>SW1(config-if)#exit
>```
>Volvemos a activar las interfaces de todo el switch, menos la interfaz 1, que se ignora según las indicaciones de la guía del proyecto.
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
>SW1(config)#exit
>SW1#copy running-config startup-config 
>Destination filename [startup-config]? 
>Building configuration...
>[OK]
>```
>Este proceso se repite en cada uno de los cuatro switches de la LAN de Campus. El resultado final puede verse pasando el mouse sobre uno cualquiera. La ventana emergente, en la que podemos comprobar rápidamente la asignación de las VLAN, se ve así:
>
>![Terminal PC](/pics/vistaswitchcampus.png)
>
>**Configuración del router:**
>
>Una vez hecha la configuración básica, encendemos la interfaz del router conectada a la LAN.
>```
>R1(config)#interface fa0/1
>R1(config-if)#no shutdown
>R1(config-if)#exit
>```
>Ahora, se crean las subinterfaces (una por VLAN usada), se les asigna protocolo de encapsulamiento y se configura su dirección IP. Encima de esto, se configura el comando de IP helper-adress con la dirección IP del servidor DHCP para que el encapsulamiento permita que las diferentes VLAN accedan al este en sus peticiones de dirección.
>```
>R1(config)#interface fa0/1.35
>R1(config-if)#encapsulation dot1q 35
>R1(config-if)#ip address 191.55.1.1   255.255.255.0
>R1(config-if)#ip helper-address 191.55.4.2
>R1(config-if)#interface fa0/1.20
>R1(config-if)#encapsulation dot1q 20
>R1(config-if)#ip address 191.55.2.1   255.255.255.0
>R1(config-if)#ip helper-address 191.55.4.2
>R1(config-if)#interface fa0/1.40
>R1(config-if)#encapsulation dot1q 40
>R1(config-if)#ip address 191.55.3.1   255.255.255.0
>R1(config-if)#ip helper-address 191.55.4.2
>R1(config-if)#interface fa0/1.55
>R1(config-if)#encapsulation dot1q 55
>R1(config-if)#ip address 191.55.4.1   255.255.255.0
>R1(config-if)#ip helper-address 191.55.4.2
>R1(config-if)#interface fa0/1.99
>R1(config-if)#encapsulation dot1q 99 native
>R1(config-if)#ip address 191.55.0.1   255.255.255.0
>```
>Por último, configuramos la dirección IP de la interfaz del router que se conecta con la red WAN, sin olvidarnos de encenderla.
>```
>R1(config)#interface Serial0/3/0
>R1(config-if)#ip address 12.130.7.2   255.255.255.192
>R1(config-if)#no shutdown
>R1(config-if)#exit
>```
>Hecho esto, podemos guardar la configuración.
>```
>R1(config)#exit
>R1#copy running-config startup-config 
>Destination filename [startup-config]? 
>Building configuration...
>[OK]
>```
>Para comprobar rápidamente que la configuración haya sido exitosa, podemos pasar el mouse sobre el router unos segundos, y así ver sus subinterfaces y direcciones asignadas. El router 1 en este proyecto tiene la siguiente ventana:
>
>![Terminal PC](/pics/vistarouter1.png)
>
>
> **Configuración de los PCs:**
> 
>Dado que ahora se tiene un servidor DHCP configurado, lo único que se necesita para que los computadores reciban una dirección DHCP es activar esta opción en su apartado de IP configuration. El resultado puede verse a continuación.
>
>![Terminal PC](/pics/vistapcoficinas.png)
>
>## **Oficinas**
> **Configuración del Multilayer Switch:**
>
>La mayor diferencia de configuración del Mutilayer Switch respecto a los switches de la LAN campus, es la asignación de puertos. En este caso, se configuraron 3 puertos en modo trunk y 2 puertos en modo acceso, ambos haciendo uso de la VLAN 200, aunque los primeros la usan como nativa.
>
>A continuación puede verse la configuración realizada.
>```
>MLSW(config)#ip default-gateway 192.168.205.1
>MLSW(config)#interface range fa0/1
>MLSW(config-if-range)#switchport mode trunk
>MLSW(config-if-range)#switchport trunk native vlan 200
>MLSW(config)#interface range fa0/2
>MLSW(config-if-range)#switchport mode trunk
>MLSW(config-if-range)#switchport trunk native vlan 200
>MLSW(config)#interface range fa0/3
>MLSW(config-if-range)#switchport mode trunk
>MLSW(config-if-range)#switchport trunk native vlan 200
>MLSW(config)#vlan 5
>MLSW(config-vlan)#name Users
>MLSW(config-vlan)#vlan 200
>MLSW(config-vlan)#name Admin
>MLSW(config)#interface range fa0/10
>MLSW(config-if-range)#Switchport access vlan 200
>MLSW(config-if-range)#exit
>MLSW(config)#interface range fa0/11
>MLSW(config-if-range)#Switchport access vlan 200
>MLSW(config-if-range)#exit
>MLSW(config)#interface vlan 200
>MLSW(config-if)#ip address 192.168.205.100   255.255.255.0
>MLSW(config-if)#exit
>MLSW(config)#exit
>MLSW#copy running-config startup-config 
>Destination filename [startup-config]? 
>Building configuration...
>[OK]
>```
>A continuación puede verse una comprobación rápida de las VLAN asignadas:
>
>![Terminal PC](/pics/vistaswitchofic.png)
>
>**Configuración del router:**
>
>La configuración del router 2 es exactamente igual a la del router 1 encontrado en el campus, tan solo cambiando las direcciones e interfaces pertinentes. A continuación puede verse la configuración hecha en este dispositivo:
>```
>R2(config)#interface fa0/1
>R2(config-if)#no shutdown
>R2(config-if)#exit
>R1(config)#interface fa0/1.5
>R1(config-if)#encapsulation dot1q 5
>R1(config-if)#ip address 192.168.10.1   255.255.255.0
>R1(config-if)#ip helper-address 192.168.205.50
>R1(config-if)#interface fa0/1.200
>R1(config-if)#encapsulation dot1q 200 native
>R1(config-if)#ip address 192.168.205.1   255.255.255.0
>R1(config-if)#ip helper-address 192.168.205.50
>R1(config-if)#exit
>R1(config)#interface Serial0/3/0
>R1(config-if)#ip address 9.9.6.17   255.255.255.252
>R1(config-if)#no shutdown
>R1(config-if)#exit
>R1(config)#exit
>R1#copy running-config startup-config 
>Destination filename [startup-config]? 
>Building configuration...
>[OK]
>```
>La comprobación rápida de esta configuración puede verse a continuación.
>
>![Terminal PC](/pics/vistarouter2.png)
>
>**Configuración del WLC:**
>
>Para configurar el WLC, hay que, primero, ingresar su dirección IP en la pestaña de navegador que ofrece packet tracer desde el PC_Admin, que previamente ha recibido su dirección IP del servidor DHCP.
>
>Después de realizar una configuración inicial, se vuelve a ingresar a la página cambiando el protocolo por HTTPS. La pantalla resultante se ve así:
>![Terminal PC](/pics/wlc1.png)
>
>Una vez ingresadas las credenciales, lo primero que hacer para crear una WLAN que se pueda asignar a diferentes puntos de acceso livianos es crear una interfaz que la controle. Para esto, nos dirigimos a la pestaña de "Controller", y una vez dentro a la sección de "Interfaces". La pantalla deseada es la siguiente:
>![Terminal PC](/pics/wlc2.png)
>
>Una vez hecho esto, se configura la interfaz con la información de la VLAN 5. Esta información se ve a continuación:
>![Terminal PC](/pics/wlc3.png)
>
>Con la interfaz creada, nos dirigimos a las pestaña de WLAN para crear la WLAN a la que se conectarán los dispositivos wireless. La pantalla que se nos presenta se ve a continuación:
>![Terminal PC](/pics/wlc4.png)
>
>Agregamos una nueva WLAN y la configuramos con todos los requerimientos del proyecto, los cuales se ven a continuación. Primero, se especifica SSID de la red, se activa, y se le asigna la interfaz creada anteriormente como controlador:
>![Terminal PC](/pics/wlc5.png)
>
>Se asigna seguridad WPA2, y se especifica una llave compartida (PSK) para el acceso, la cual será "Cisco123":
>![Terminal PC](/pics/wlc6.png)
>![Terminal PC](/pics/wlc7.png)
>
>Por último, activamos las opciones de _FlexConnect Local Switching_ y _FlexConnect Local Auth_:
>![Terminal PC](/pics/wlc8.png)
>
>Así, ha quedado configurada la red WLAN que va a manejar el Lightweight Access Point.
>
>**Configuración del LAP:**
>
>Para configurar el Lightweight Access Point lo único necesario es permitir que reciba su dirección por medio del servidor DHCP en su interfaz de Management y especificar la dirección IP del WLC que lo maneja.
>
>La comprobación rápida de estas configuraciones se ve así:
>![Terminal PC](/pics/vistalap.png)
>
> **Configuración de los hosts wireless:**
> 
>Puesto que ya se configuró la dirección IP que debería recibir cada dispositivo basado en que la red WLAN pertenece a la VLAN 5, lo único que hay que hacer es ingresar el SSID y contraseña de la red WLAN y permitir que reciba direcciones IP a través del protocolo DHCP.
>
>El resultado puede verse a continuación:
>
>![Terminal PC](/pics/vistawirelessofi.png)
>
> ## **Enrutamiento**
>
>Para todos los routers de la toplogía se configuró el protocolo de enrutamiento RIPv2.
>
>Para esto se configuró en cada router la version del protocolo usada, las diferentes direcciones que conoce en cada una de sus interfaces, y desactivar la opción de auto-summary, que podría desconfigurar el protocolo en redes WAN pequeñas.
>
>El proceso de configuración puede verse ejemplificado a continuación, usando el router ISP:
>```
>ISP(config)#router rip
>ISP(config-router)#version 2
>ISP(config-router)#no auto-summary
>ISP(config-router)#network 9.9.6.16
>ISP(config-router)#network 12.130.7.0
>ISP(config-router)#network 12.130.7.64
>ISP(config-router)#network 209.175.105.1
>```


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
>Para verificar la conectividad debemos hacer un `ping` al IP del PC en la misma VLAN, para este caso usamos la de biblioteca, por lo que usaremos los PC  8 y 12. Desde el PC 8 en la command prompt hacemos `ping 190.35.1.4` (IP del PC 12)
>
>![Terminal PC](/pics/img_InterVLAN2.png)
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
>El paquete salió del PC1 y tiene como destino el PC5, como se puede ver, ambos PCs pertenecen a la misma VLAN pues sus direcciones IP están dentro del rango de IP ‘190.35.4.x’, el cual corresponde a la VLAN 55. 
>
>El paquete paso por nodos intermedios, llego al switch 1, posteriormente al switch 2, y luego al PC5. Como se puede ver también se envió un paquete al PC9 debido a que este pertenece a la misma VLAN, pero este fue rechazado.
>
>![Terminal PC](/pics/imagen_an_2.png)
>
>Al Salir del PC4 se evidencia que el paquete esta siendo devuelto al PC1 pues se indica la Dirección Mac Destino, la cual coincide con la del PC1
>
## Conectividad con la puerta de enlace:
>![Terminal PC](/pics/imagen_an_3.png)
>
>Como se evidencia aqui, el paquete fue enviado por el PC12, dirigido a la dirección IP de la default gateway. Posteriormente viaja a el switch 4, luego al switch 3 y finalmente al router. Luego toma el mismo camino de vuelta al PC12, así comprobando la conexión entre el PC12 y la default gateway.
>
## Conectividad entre dos PCs en VLANs distintas:
>![Terminal PC](/pics/imagen_an_4.png)
>
>En este caso se esta haciendo ping desde el PC3 al PC12 puesto que estos pertenecen a la VLAN 35 y a la VLAN 20 respectivamente. 
>
>Inicialmente se lleva a cabo el protocolo ARP, por lo cual se envia un mensaje broadcast por todas las VLANs para encontrar la dirección MAC del PC12, por esta razón se pierde el primer paquete.
>
>![Terminal PC](/pics/imagen_an_5.png)
>
>Posteriormente, se comienzan a enviar solamente mensajes de peticion respuesta, puesto que, gracias al protocolo ARP, ya fue encontrada la direccion MAC del PC12. Estos paquetes son todos enviados y recibidos de manera exitosa, por lo que se comprueba la conexion entre el PC3 y el PC12.

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

Uno de los grandes desafíos que afrontamos como equipo fue la falta de conocimiento con respecto a la implementación práctica de los diferentes conceptos vistos en clase. Dedicamos bastante tiempo a la investigación de la configuración de ciertos dispositivos, en especial la red LAN de Oficinas y sus dispositivos de caracter wireless, a fin de entender y replicar las configuraciones que se nos pedían para el proyecto.

Otro reto muy importante fue las carencias que tiene Packet Tracer como software colaborativo, puesto que para que todos pudieramos contribuir al proyecto teníamos que esperar a que los demás finalizaran sus cambios por completo y facilitaran una copia de su archivo de trabajo.


# Conclusiones:

Sobre todo, este proyecto nos permitió afianzar nuestros conocimientos acerca del desarrollo de redes de comunicación de datos. Mediante los diferentes retos asignados, se nos puso a prueba no solo con respecto a la configuración de los diferentes conceptos que vimos a lo largo del curso, sino que se nos forzó a desarrollar habilidades de resolución de problemas respecto a lo que redes de comunicación de datos respecta.

Gracias a estas habilidades fue que fuimos capaces de resolver los problemas que se nos presentaban a medida que desarrollabamos el proyecto, y ahora somos capaces de identificar las principales causas de problemas y como resolverlas.


# Bibliografía:

-[1] J. Aranda, Cisco Switch - Basic Configuration. 2022.

-[2] Sunny Classroom. Spanning Tree Protocol (IEEE 802 1D). (1 de julio de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=Ilpmn-H8UgE

-[3]Sunny Classroom. How STP Elects Root Bridge with Hello BPDU? (7 de julio de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=BkGEwrzIK4g



