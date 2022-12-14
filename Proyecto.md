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
>
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
>
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
>
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
>Asignamos las interfaces, a las respectivas VLANs basados en las indicaciones de la tabla presentada en la guía del proyecto.
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
>Finalmente, guardamos la configuración.
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
> **Configuración Servidor DHCP:**
>Para el servidor DCHP debemos ponerle de dirección estatica la dirección que en el router pusimos como ip-helper-address y la default gateway es la que interfaz del router que se asigna, en nuestro caso como el servidor se encontraba en la VLAN 55, teniamos que darle la default gateway correspondiente.
>En la sección de servicios apagabamos todos excepto DHCP, en esta configurabamos por VLAN las IPs que podian tener, su default gateway, nombre, cantidad de usuarios y el servidor DNS con el que va a traducir las IPs a URLs.
>![Terminal PC](/pics/dhcp_pool.png)
>Aunque para este caso donde nuestro servidor hace parte de una de las VLANs a la que otros computadores confian para que se les asigne IP dinamicamente, toca tener en cuenta el server pool, este es en el que se encuentra nuestro servidor por esto mismo no tenemos VLAN 55, ya que el server pool hace de este, ademas que tiene un usuario menos, y una IP inicial mas arriba, ya que no puede asignar a partir del 2.
>
>
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
>
>![Terminal PC](/pics/vistalap.png)
>
> **Configuración de los hosts wireless:**
> 
>Puesto que ya se configuró la dirección IP que debería recibir cada dispositivo basado en que la red WLAN pertenece a la VLAN 5, lo único que hay que hacer es ingresar el SSID y contraseña de la red WLAN y permitir que reciba direcciones IP a través del protocolo DHCP.
>
>El resultado puede verse a continuación:
>
>![Terminal PC](/pics/vistawirelessofi.png)
> ## **Mi casa "Inteligente"**
> Para la casa inteligente, debemos de configurar el router ISP para que este le de el direccionamiento dinamico (DHCP) a nuestros dispositivos, todo esto mediante unas lineas de comandos.
>```
>ISP(config)#
>ISP(config)#ip dhcp pool WAN
>ISP(dhcp-config)#network 12.130.7.64 255.255.255.192
>ISP(dhcp-config)#default-router 12.130.7.65
>ISP(dhcp-config)#dns-server 209.175.105.3
> ```
> Por parte del cloud es por el concepto, de lo que esta ocurriendo, si lo vemos desde otra perspectiva, el ISP y los servidores es toda la parte con la que nunca interactua el usuario, es el internet, el como cientos o miles de routers se interconectan para terminar llegando a en nuestro caso la casa inteligente. Por parte del cloud lo unico que teniamos que configurar era la entrada de el proveedor de la red, que es por cable, y en las conexiones toca especificar cual es la conexión por donde sale y por donde entra. Desde la entrada que es Coaxial 7 al puerto Ethernet 6.
>![Terminal PC](/pics/Cable_Cloud.png)
>Por otra parte, tenemos que configurar el Home Gateway donde le ponemos una IP, que termina siendo la default gateway de todos los dispositivos. Aunque por Internet puede obtener una IP por DHCP, dada por el ISP. En cada uno de los dispositivos toca especificar que la IP sera dinamica, ademas en la sección de Wireless, el SSID del Home Gateway y el servidor al que IoT server se va a albergar, tambien es el Home Gateway. En un hipotetico caso donde dispositivos de otra red, quieran acceder a este, el servidor deberá ser remoto.
>![Terminal PC](/pics/iot.png)
>
> ## **Servidores**
>
>Para la configuración de la LAN de servidores, el enrutamiento no fue ningún problema, y no hubo que hacer más que otorgar direcciones estáticas a la interfaz del ISP conectada a la LAN y a cada uno de los servidores. 
>
>Vale la pena mencionar que las direcciones IP utilizadas en esta LAN provienen directamente de la primera red disponible después de realizar el proceso de subneteo con el rango especificado en la guía del proyecto.
>
>La configuración de los servidores se ve a continuación:
>
> **Configuración del servidor HTTP:**
>
>En cuanto a la configuración del servidor HTTP, aparte de asignarle una dirección, máscara de subred y puerta de enlace predeterminada fija, hubo que encender el servicio HTTP, apagar todos los demás, y añadir los archivos HTML deseados para generar la página.
>
>Así, ingresando la dirección IP que se le asignó, un usuario puede acceder a la página hosteada por el servidor desde el internet.
>
>A continuación puede verse una imagen de la configuración del servicio HTTP:
>
>![Terminal PC](/pics/http.png)
>
> **Configuración del servidor DNS:**
>
>De forma similar al servidor HTTP, antes de configurar el servicio DNS le asignamos de forma estática al servidor una dirección IP, una máscara de subred y una puerta de enlace predeterminada. Además de esto, se apagaron los demás servicios y, en el servicio de DNS, se añadió una entrada que permitiera traducir desde el nombre *JJC_JDG_CFG.net* a la dirección IP del servidor HTTP determinada en el paso anterior.
>
>Así, y una vez especificado como servidor DNS en un host, se puede acceder al servicio para transformar un nombre a una dirección IP y navegar más fácilmente por el internet..
>
>A continuación puede verse una imagen de la configuración del servicio DNS:
>
>![Terminal PC](/pics/DNS.png)
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
>
>Vale la pena mencionar en este apartado que las redes que utilizamos para el router ISP son:
>- 209.175.105.1: Primera red disponible del subneteo realizado para la red LAN de servidores.
>- 9.9.6.18: Determinada por realizar el proceso de subneteo inverso dada la dirección del router 2. Básicamente, es la única dirección disponible dentro del rango al que pertenece la dirección dispuesta en el enlace serial del router 2.
>- 12.130.7.1 y 12.130.7.65: Determinadas por el subneteo WAN y asignadas a los puertos conectados a las redes restantes (Campus y Casa Inteligente).

# Verificaciones

>### Comunicación entre los usuarios de la misma VLAN en la red “Campus”  
>
>Debemos de ir al command prompt en el desktop del PC deseado, usaremos la VLAN 55, desde el PC1 haremos `ping` a la IP dinamica del PC 5, que se encuentra en la misma VLAN, en este caso debemos revisar la IP del otro computador antes ya que esta es asignada automaticamente, puede variar. 
>
>![Terminal PC](/pics/interVlan.png)
>
> ### Comunicación entre los usuarios pertenecientes a VLANs diferentes en la red “Campus”
>
>Repetimos el mismo procedimiento de usar el comando `ping` pero a partir de el PC3, que se encuentra en la VLAN 20, conectaremos al PC 12 en la VLAN 35. Como ya mencionamos la IP del dispositivo al que le realizamos el ping puede cambiar, por lo que no siempre este PC tendra esa misma IP. 
>
>![Terminal PC](/pics/intraVlan.png)
>Como es de esperarse se pierde un paquete por el uso de ARP, pero esto no significa un problema con la conexión.
>
> ### Soportar el protocolo PVRST (Per VLAN Rapid Spanning Tree) y 802.1Q trunking en las LANs
>
>Los switches les agregamos el PVRST, el encapsulamiento mediante 802.1Q es en el router, y se configuro por cada una de las VLAN's
>
>![Terminal PC](/pics/RapidPerVlanSpaningTree.png)
>![Terminal PC](/pics/Encapsulamiento.png)
>
>
>
> ### Soportar el enrutamiento RIPv2 en las interfaces de routers requeridos. 
>
>Cada uno de los routers del proyecto cuenta con el enrutamiento RIPv2, aignando diferentes redes en las interfaces y el serial de conexión.
>
>![Terminal PC](/pics/R1_RIP.png)
>![Terminal PC](/pics/R2_RIP.png)
>![Terminal PC](/pics/ISP_RIP.png)
>
> ### Acceso a la pagina web personalizada, con nombre de dominio proporcionado por el DNS
>
>Todos los dispositivos en la red pueden acceder a la pagina **JJC_JDG_CFG.net**  el cual es un domonio del DNS, pero en realidad es la IP de donde se encuentra alojada nuestra pagina web con HTML respectivo. 
>
>![Terminal PC](/pics/webOficinas.jpeg)
>![Terminal PC](/pics/webCampus.jpeg)
>![Terminal PC](/pics/webCasa.jpeg)
>
>
>
> ## Acceso a dispositivos IoT desde nodos terminales
> Los dispositivos dentro de la red de "Mi casa inteligente" tienen acceso desde Desktop a IoT monitor donde pueden controlar los dispositivos del internet de las cosas.
>![Terminal PC](/pics/IOT_working.png)
>![Terminal PC](/pics/iot_cel.png)
> ## Script 
> Script corre del lado del cliente, y del lado del servidor.
>![Terminal PC](/pics/socket.png)

# Análisis de tráfico:
## Acceso a la pagina web personalizada desde campus, casa inteligente y oficinas, con nombre del dominio proporcionado por el DNS:
> ### Acceso desde el campus 
>
>![Terminal PC](/pics/Pasted_Graphic_1.png)
>
>Como se puede ver en esta imagen, viaja a lo largo de la red un paquete, que sale del PC1 tras ingresar la URL en el navegador, pasa por el switch 1, luego llega al switch 3, luego al router 1, y después al ISP, de aquí pasa al switch 0 de la LAN de los servidores, luego pasa por el servidor DNS y vuelve hasta el PC1. 
>
>En la siguiente imagen se observa el paquete justo después de pasar por el DNS, en rojo esta el DNS query y en azul la respuesta, como se puede ver, la respuesta lleva la dirección IP del servidor web.
>
>![Terminal PC](/pics/Pasted_Graphic_2.png)
>
>Una vez el trabajo del protocolo DNS se ha completado, todo lo que queda son una serie de paquetes de petición y respuesta HTTP entre el PC y el servidor web.
>
> ### Acceso desde la casa inteligente 
>
>![Terminal PC](/pics/Pasted_Graphic_3.png)
>
>Desde la casa inteligente también se puede acceder. En este caso, primero se lleva a cabo el protocolo DNS, aquí se envia un paquete inicialmente al Home Gateway, el cual luego lo envia un mensaje Broadcast el cual es rechazado por todos los dispositivos que comprueben que no son el destinatario. Después de esto el proceso es igual al anterior, el paquete llega al servidor DNS, el DNS devuelve una respuesta y la Laptop obtiene la dirección IP del servidor Web. Posteriormente envia el request de HTTP y todos los paquetes con información comienzan a ser enviados del servidor a la Home Gateway, que los vuelve a enviar en mensajes Broadcast, y finalmente llega a la Laptop.
>
>![Terminal PC](/pics/Pasted_Graphic_4.png)
>
> ### Acceso desde la oficina
>
>![Terminal PC](/pics/Pasted_Graphic_5.png)
>
>También se puede acceder desde la oficina. El proceso es casi idéntico al de la casa inteligente, la diferencia en este caso es que los paquetes que llegan al LAP desde uno de los dispositivos conectados no son distribuidos mediante un mensaje Broadcast, sino que el paquete llega directamente al destinatario.
>
## Acceso a dispositivos IoT desde nodos terminales:
>![Terminal PC](/pics/Pasted_Graphic_6.png)
>
>El dispositivo  envia un http request a la Home Gateway para hacer un upgrade la conexión a un Web Socket
>
>![Terminal PC](/pics/Pasted_Graphic_7.png)
>
>La Home Gateway envia un http request a todos los dispositivos para hacer el upgrade
>
>![Terminal PC](/pics/Pasted_Graphic_8.png)
>
>Posteriormente comienza a enviar mensajes Broadcast con la laptop como destinatario. 
>
>![Terminal PC](/pics/Pasted_Graphic_9.png)
>
>Al hacer cambios o cambiar el estado de los dispositivos IoT, la laptop envía un mensaje al Home Gateway y el siguiente mensaje Broadcast de este, realiza el cambio. Sin embargo, estos mensajes no van en forma de request o respuesta HTTP, solo llegan información, y esto pasa debido a que la conexión es de WebSocket. 
>
## Script:
>![Terminal PC](/pics/Pasted_Graphic_10.png)
>
>Al ejecutar el scripten socket TCP en el PC1 y al conectarse con el servidor socket-TCP corriendo en el Servidor Web vemos que el flujo comienza estableciendo una conexión directa entre el PC1 y el Servidor Web. Posteriormente se comienzan a enviar mensajes de ida y vuelta por medio del protocolo TCP.
>


# Protocolos:

Los protocolos de comunicación de datos son una serie de raglas vitales que dictan la forma en la que la comunicación de datos se lleva a cabo, y especifican los pasos y métodos a seguir por los diferentes paquetes para cumplir su objetivo en la red.

A continuación presentamos la desripción de los protocolos más importantes que usamos en el proyecto.

> **-IPv4:** Probablemente el protocolo más importante de toda nuestra red, consiste en definir una serie de direcciones de 32 bits para la transmisión organizada de información a través de redes físicas. Actualmente, y debido al enorme crecimiento del Internet en los últimos tiempos, se ha desarrollado otro protocolo conocido como IPv6, que tiene un número de direcciones totales que superan por órdenes de magnitud a las que puede generar en total el IPv4, sin embargo, para la red propuesta en el proyecto este protocolo es más que suficiente.
>
> **-DHCP:** Mediante este protocolo, un host es capaz de asignar a cada equipo dentro de su red LAN una IP única para su navegación por internet. Se realiza de manera automática y dinámica a todos los dispositivos que se conectan con la red y generan una solicitud, y aparte de dirección IP asigna gateways predeterminadas, máscaras de subred y otros parámetros de red.
>
> **-DNS:** Este protocolo se encarga de transformar un “nombre” o dirección de una página web al número IP bajo el que realmente está identificado ese dominio. Básicamente, permite relacionar IP’s con nombres para que nosotros los usuarios podamos memorizar solo los nombres y olvidar las IP’s, facilitando la navegación web.
>
> **-HTTP:** Este protocolo permite la transferencia de datos a través de archivos a través de la red. Básicamente, permite que la información de las páginas web montadas en servidores puedan ser accedidas desde la red mediante esquemas de petición-respuesta entre clientes y servidores.
>
> **-WPA2-PSK:** Uno de los muchos protocolos de seguridad informática. Como todo protocolo de seguridad, se encarga de establecer las claves que se utilizarán para cifrar la comunicación entre punto de acceso y el terminal del usuario. WPA2 es un protocolo que soluciona las vulnerabilidades tanto de su antecesor, el WPA, como del primer algoritmo de seguridad, el WEP. Utiliza un método de encriptación más sofisticado conocido como AES (Advanced Encryption Standard) que el TKIP, que venían utilizando sus antecesores. Esta versión en específico, notada por terminar en -PSK (Pre-Shared Key) tiene una clave precompartida en vez de una clave diferente para cada usuario como en su versión original, y la escogimos puesto que es la más cómoda para las redes de hogar en las que todos los miembros conocen la misma frase contraseña.
>
> **-STP:** A grandes rasgos, este protocolo tiene como propósito evitar las tormentas de multidifusión, momentos en los cuales un mensaje broadcast queda atrapado en un bucle dentro de la red, y genera tráfico de forma perpetua.
>Para conseguirlo, este protocolo se aplica a cualquier ciclo que pueda causar esto, en nuestro caso, se aplica al ciclo creado por los 3 switches, y consiste en escoger un switch principal, conocido como “Puente raíz”, que será el de menos ID de prioridad y MAC, y luego permitir que, a nivel lógico, se mantengan activos solo los puertos que forman parte de la ruta de menor costo desde cualquier otro switch al puente raíz. Finalmente, todos aquellos puertos que tienen una conexión que no es usada en ninguna ruta de coste menor son cerrados desde uno solo de los extremos, aquel que esté en el dispositivo de menor ID de prioridad y MAC.
>
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

Por otra parte aunque ya hemos manejado teoricamente conceptos como el DHCP, no los habiamos implementado y las guias en internet eran muy amplias, y en el proyecto teniamos requerimientos muy especificos, lo que hizo que comprender donde fallaba era mucho mas dificil.

Otro reto muy importante fue las carencias que tiene Packet Tracer como software colaborativo, puesto que para que todos pudieramos contribuir al proyecto teníamos que esperar a que los demás finalizaran sus cambios por completo y facilitaran una copia de su archivo de trabajo.

El último reto significativo que afrontamos fue el uso de la herramienta de simulación en Packet Tracer, puesto que intentar entender la información que portan y transmiten los paquetes en cada punto diferente de la red se convierte en una tarea muy compleja cuando se intenta reconocer cada protocolo que está interfiriendo en la comunicación de datos y cuales son los cambios puntuales que generan en los paquetes enviados.

# Conclusiones:

Sobre todo, este proyecto nos permitió afianzar nuestros conocimientos acerca del desarrollo de redes de comunicación de datos. Mediante los diferentes retos asignados, se nos puso a prueba no solo con respecto a la configuración de los diferentes conceptos que vimos a lo largo del curso, sino que se nos forzó a desarrollar habilidades de resolución de problemas respecto a lo que redes de comunicación de datos respecta.

Gracias a estas habilidades fue que fuimos capaces de resolver los problemas que se nos presentaban a medida que desarrollabamos el proyecto, y ahora somos capaces de identificar las principales causas de problemas y como resolverlas.

Desarrollar una red tan robusta, con redes de redes nos permite ver desde otra perspectiva como funciona el internet, claramente a una menor escala, pero nos damos a una idea más asentada en la realidad que nos permite comprender el montaje y topología general de otros tipos de sistemas con mucha mas facilidad. 

Por último, este proyecto nos ayudó en gran medida a entender y deducir el funcionamiento específico de los múltiples protocolos de red vistos cuando dejan de estar aislados y se juntan los unos con otros en una red WAN compleja.

# Bibliografía:

-[1] J. Aranda, Cisco Switch - Basic Configuration. 2022.

-[2] Sunny Classroom. Spanning Tree Protocol (IEEE 802 1D). (1 de julio de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=Ilpmn-H8UgE

-[3]Sunny Classroom. How STP Elects Root Bridge with Hello BPDU? (7 de julio de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=BkGEwrzIK4g

-[4] Concepto. 2021. Protocolo Informático - Concepto, propiedades y ejemplos. [online] Available at: <https://concepto.de/protocolo-informatico/>.

-[5] De León Guerrero, E., 2016. Redes inalámbricas WPA/WPA2 ¿La protección ya no es suficiente? | Revista .Seguridad. [online] Revista.seguridad.unam.mx. Available at: <https://revista.seguridad.unam.mx/numero-19/redes-inalambricas-wpawpa2-la-proteccion-ya-no-es-suficiente>.

-[6] Calderon, C., Calderon, C. and Calderon, C., 2021. ¿Qué son los dispositivos de Red y para qué sirven?. [online] Redes de Computadores. Available at: <https://www.plotandesign.com/redes/dispositivos-de-red/>.

-[7] Sepúlveda, M., 2022. Armando un Servidor en Cisco Packet Tracer - eClassVirtual - Cursos Cisco en línea. [online] eClassVirtual - Cursos Cisco en línea. Available at: <https://eclassvirtual.com/armando-un-servidor-en-cisco-packet-tracer/>.

-[8] L. Peterson, and  B. Davie, Computer Network: a systems approach, 5ed. Burlington, USA: Elsevier, 2012, Ch 8. 

-[9] Walton, A., 2022. Cable Directo, Cable Cruzado y Cable Consola ¿Cuáles son las diferencias?. [online] CCNA. Available at: <https://ccnadesdecero.es/cable-directo-cruzado-y-consola-diferencias/>.

-[10]Configurar.pro. 2022. ▷ ¿Como Configurar Servidor Web Packet Tracer? 【 agosto 2022 】 Configurar.pro. [online] Available at: <https://configurar.pro/servidores/como-configurar-servidor-web-packet-tracer>.

-[11]R. Avellaneda. "ShieldSquare Captcha". ShieldSquare Captcha. https://networklessons.com/cisco/ccna-200-301/cisco-wireless-lan-controller-wlc-basic-configuration.

-[12]RKiLAB. Configuring Wireless LAN controller using Radius & DHCP server for enable WPA&WPA2 security.(1 de diciembre de 2019). [Video en línea]. Disponible: https://www.youtube.com/watch?v=TKhSIdyBbHY&t=1s
