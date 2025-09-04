# Portafolio-An-lisis-de-Seguridad-en-Redes-de-Datos

Introducción 
El objetivo de este análisis es evaluar el comportamiento y la seguridad de una red mediante el uso de herramientas especializadas en escaneo y análisis de tráfico. Utilizando Kali Linux como entorno principal, se realizaron pruebas activas con hping3, enviando distintos tipos de paquetes a hosts remotos para analizar respuestas del sistema y posibles configuraciones de seguridad. Además, se utilizó Wireshark para capturar y analizar el tráfico de red generado durante sesiones de tráfico, permitiendo identificar protocolos, puertos y posibles vulnerabilidades presentes.


Desarrollo 
1. Análisis de Tráfico con hping3 en Kali Linux  
● a) Crear un archivo de texto plano con información personal (nombre, curso, fecha)  
Creamos un archivo con el editor de texto vi con el nombre INFO_PERSONAL.txt 
En el editor de texto vi agregamos los datos correspondientes; nombre, curso y fecha
Como se puede observar el archivo fue creado correctamente con la información solicitada 
● b) Utilizar hping3 para enviar diferentes tipos de paquetes a un host 
objetivo (puede ser google.com o la puerta de enlace local):  
En este caso se utilizó la página google.com como host objetivo para enviar los diferentes paquetes.
○ ICMP ping normal  
Como se puede observar en la captura de wireshark, se capturaron paquetes ICMP, echo Request  enviados por la ip 10.0.2.15 correspondiente a la máquina kali y paquetes echo Reply qué son las respuestas de parte de la ip 142.250.0.101 correspondiente a google.com. 
○ TCP SYN a puerto 80  
Como se puede observar en la captura de wireshark, se generó tráfico TCP al puerto 80, desde la ip 10.0.2.15 correspondiente a la maquina kali solicitando 
establecer una conexión TCP con la ip 142.250.0.113 correspondiente a google.com a traves de un TCP SYN. Y se puede observar que google.com respondió el TCP SYN con un SYN,ACK lo que nos permite concluir que el puerto 80 se encuentra abierto. 
○ UDP a puerto 53  
Como se puede observar en la captura de wireshark, se generó tráfico UDP desde la ip 10.0.2.15 a la ip 10.0.2.3 a través de DNS para resolver el nombre de dominio de google.com a dirección ip para realizar el envío de paquetes. 
También se generó tráfico UDP desde la ip 10.0.0.2.15 a la ip 142.250.0.102 
correspondiente a google.com , y como se puede observar no hay respuesta por parte de google.com. ○ TCP con datos personalizados  
Como se puede observar en la captura de wireshark, se generó tráfico TCP al puerto 80, desde la ip 10.0.2.15 correspondiente a la maquina kali solicitando establecer una conexión TCP con la ip 142.250.0.113 correspondiente a google.com a traves de un TCP SYN con payload, dicho payload se puede observar en la captura como fue transmitido en texto plano. 
● c) Documentar los comandos utilizados y explicar las diferencias en las 
respuestas  
El comando utilizado para enviar un paquete ICMP fue el siguiente: 
sudo hping3 -1 -c 4 google.com    
Este comando utiliza el protocolo ICMP y envía 4 paquetes echo request a 
google.com, similar al comando ping. Google respondió con echo reply, lo que indica que hay conectividad entre la máquina kali y google.com , y que este acepta paquetes ICMP. 
El comando utilizado para enviar un paquete TCP SYN al puerto 80 fue el siguiente: 
sudo hping3 -S -p 80 -c 4 google.com     
Este comando usa el protocolo TCP y se encarga de enviar 4 paquetes TCP SYN al puerto 80 de google.com , simulando el inicio de una conexión HTTP.
google.com respondió estos TCP SYN con SYN,ACK lo que nos da entender que el puerto 80 de google.com se encuentra abierto y respondiendo solicitudes de establecimiento de conexión. 
El comando utilizado para enviar un paquete UDP al puerto 53 fue el siguiente: 
sudo hping3 -2 -p 53 -c 4 google.com    
Este comando utiliza UDP para enviar 4 paquetes al puerto 53 (usado comúnmente para DNS) de google.com. En este caso, no se recibió ninguna respuesta. Esto podría significar que el puerto está cerrado, filtrado por un firewall o que simplemente el servidor no responde a paquetes UDP. 
El comando utilizado para enviar un paquete TCP con datos personalizados al puerto 80 fue el siguiente: 
sudo hping3 -S -p 80 -d 100 -E INFO_PERSONAL.txt google.com 
Este comando utiliza el protocolo TCP y se encarga de enviar paquetes TCP SYN al puerto 80 con un payload de 100 bytes de datos personalizados del archivo INFO_PERSONAL.txt. Esto se puede usar para probar cómo reacciona un servidor a ciertos contenidos de paquetes. En este caso, hubo respuesta SYN,ACK por parte de google.com a los paquetes TCP SYN y también se puede observar que el contenido del paquete se transmite en texto plano. 
3. Captura y Análisis de Tráfico con Wireshark  
Realizar una sesión de captura de tráfico de red durante 10-15 minutos mientras navegas por diferentes sitios web:  
● a) Acceder a al menos 5 sitios web diferentes (incluir HTTP y HTTPS)  
Se accedió a las siguientes páginas web: 
Con HTTPS : www.youtube.com , www.cisco.com , www.desafiolatam.com y www.fortinet.com  
Con HTTP : www.example.com 
● b) Realizar una descarga de archivo pequeño  
Para la descarga, se realizó una búsqueda de una imagen en el navegador y se procedió con la descarga. 
● c) Enviar un correo electrónico o usar una aplicación de mensajería  
Para el envío de correo, se utilizó el correo guillermo.pezo24@gmail.com para enviar un correo de prueba a un correo temporal. 
Como se puede observar el correo llegó a la bandeja de entrada del correo temporal. 
● d) Aplicar los siguientes filtros en Wireshark y documentar los resultados:  
○ http - Tráfico HTTP  
Como se puede observar, se capturó tráfico HTTP, se pueden ver solicitudes requests de la máquina kali y responses de la página HTTP, también se pueden observar solicitudes GET y respuestas HTTP 200 ok que indica que la solicitud ha tenido éxito, también hay respuestas HTTP 301 de redirección web. 
○ dns - Consultas DNS  
Como se puede observar, se generó bastante tráfico DNS, bastantes consultas 
DNS desde la ip 10.0.2.15 de la maquina kali hacia la 10.0.2.3 correspondiente al DNS local. Se puede observar consultas A , AAA, y en cada consulta la máquina kali recibe una respuesta. También se pueden observar diferentes dominios como desafiolatam.com , services.mozilla.com , ocsp.r2m03.amazontrust.com  y la dirección IPv4 o IPv6 correspondientes a esos dominios. 
○ tcp.port == 443 - Tráfico HTTPS  
Como se puede observar, se capturó tráfico del puerto 443, entre el tráfico se puede identificar tráfico TLS versión 1.3 , y los mensajes característicos de esta versión de TLS como client hello, server hello, application data, etc. También se pueden observar paquetes TCP con flag ACK, número de secuencia, tamaño de ventana confirmando la recepción de datos anteriores en una conexión. 
○ icmp - Tráfico ICMP  
Como se puede observar también se capturó tráfico ICMP, donde indica que, aunque el host de destino sí fue alcanzado, la mayoría de tráfico indica destination unreachable (port unreacheable), lo que indica que no había ningún servicio o aplicación escuchando en el puerto al que se intentaba enviar el paquete o este no se encuentra respondiendo solicitudes. 
○ ip.addr == [tu_IP] - Todo el tráfico de tu máquina  
Como se puede observar, se capturo todo el trafico en el que participa la ip 10.0.2.15 correspondiente a la maquina kali. Hay tráfico TCP indicando conexiones a distintas direcciones ip públicas de páginas web a través del puerto 80 y 443.También se puede observar tráfico TLS que nos indica que el flujo de datos está siendo cifrado asegurando una comunicación segura entre el 
cliente y el servidor. 
● e) Identificar y explicar:  
○ Protocolos más utilizados  
Como se observó anteriormente los protocolos más utilizados son HTTP, HTTPS, TCP, UDP, TLS y DNS. 
Ya que el tráfico capturado por wireshark fue trafico WEB que se basa en la utilización de estos protocolos, por ejemplo: 
1. Se selecciona la URL de la página https://www.google.com 
2. El DNS se encarga de resolver ese nombre de dominio a dirección IP para saber el destino → 142.250.190.4 
3. El navegador establece una conexión TCP al puerto 443 o 80 dependiendo de si es HTTPS o HTTP. 
4. En el caso de HTTPS se inicia el protocolo TLSv1.3 u otra versión para negociar los parámetros de cifrado de la comunicación. 
5. Se envía petición HTTP cifrada (ej. GET /) para solicitar información de la 
página web. 
6. El servidor responde la solicitud con la página cifrada. 
○ Direcciones IP de destino más frecuentes  
Las direcciones IP de destino más frecuentes son las siguientes: 
10.0.2.15 (Maquina Kali): Debido a que es la dirección ip de origen de las solicitudes también es la dirección ip de destino de respuesta de las solicitudes. 
10.0.2.3: IP correspondiente al DNS local para resolver nombres de dominios a direcciones IP. 
34.36.137.203: IP correspondiente a una de las páginas web visitadas. 
108.177.123.94: IP correspondiente a una de las páginas web visitadas. 
34.107.221.82: IP correspondiente a una de las páginas web visitadas. 
○ Puertos más utilizados  
Los puertos más utilizados son el puerto 80, 443 y 53 que corresponden a los protocolos HTTP, HTTPS y DNS.
○ Posibles vulnerabilidades observadas (tráfico no cifrado, etc.) HTTP presenta las siguientes vulnerabilidades: 
Tráfico no cifrado: El tráfico enviado y recibido en el navegador viaja en texto plano. Lo cual puede traer consecuencias como que cualquier persona puede interceptar el tráfico, leer los datos, modificarlos, etc. 
Falta de autenticación: Al no haber cifrado ni verificación del servidor, es difícil garantizar que el servidor es legítimo. 
Falta de integridad: Los datos enviados por HTTP pueden ser modificados en tránsito sin que el cliente o el servidor lo detecten. 
Posible blanco de ataques: Los datos al ser enviados sin cifrado se convierten en una vulnerabilidad que puede ser aprovechada para ataques de sniffing, man-in-the-middle o inyección de contenido malicioso. 
3. Análisis de Conectividad y Respuesta de Red  
Utilizando tanto hping3 como Wireshark:  
● a) Realizar un análisis de conectividad a diferentes puertos de un servidor remoto:  
○ Puerto 22 (SSH)  
Como se puede observar se envió un paquete (SYN) desde la ip 10.0.2.15  a la ip 45.33.32.156 a través de TCP puerto 22, intentando iniciar una conexión. Se puede observar una respuesta (SYN,ACK) desde la ip 45.33.32.156 a la 10.0.2.15 lo que indica que el servidor responde al intento de conexión e indica que el puerto 22 está abierto. 
○ Puerto 80 (HTTP)  
Como se puede observar se envió un paquete (SYN) desde la ip 10.0.2.15  a la ip 45.33.32.156 a través de TCP puerto 80, intentando iniciar una conexión. Se puede observar una respuesta (SYN,ACK) desde la ip 45.33.32.156 a la 10.0.2.15 lo que indica que el servidor responde al intento de conexión e indica que el puerto 80 está abierto. 
○ Puerto 443 (HTTPS)  
Como se puede observar se envió un paquete (SYN) desde la ip 10.0.2.15  a la ip 45.33.32.156 a través de TCP puerto 443, intentando iniciar una conexión. Se puede observar una respuesta (RST,ACK) desde la ip 45.33.32.156 a la 10.0.2.15 lo que indica que el servidor recibe el syn pero rechaza el intento de conexión.Esto indica que el puerto 443 lo más seguro es que se encuentra filtrado o cerrado. 
○ Puerto 21 (FTP)  
Como se puede observar se envió un paquete (SYN) desde la ip 10.0.2.15  a la ip 45.33.32.156 a través de TCP puerto 21, intentando iniciar una conexión. Se puede observar una respuesta (RST,ACK) desde la ip 45.33.32.156 a la 10.0.2.15 lo que indica que el servidor recibe el syn pero rechaza el intento de conexión.Esto indica que el puerto 21 lo más seguro es que se encuentra filtrado o cerrado. 
● b) Documentar qué puertos están abiertos, cerrados o filtrados  
Para poder confirmar si los puertos escaneados anteriormente se encuentran abiertos, cerrados o filtrados lo más recomendable es usar nmap para verificar su estado. 
Como se puede observar el puerto 22 y 80, se encuentran efectivamente 
abiertos como se había concluido en la captura de tráfico anterior.Y también se puede observar que el puerto 21 y 443 se encuentran filtrados lo que indica que no se puede determinar si el puerto se encuentra abierto o cerrado ya que no se recibió una respuesta o esta respuesta fue bloqueada por algún firewall o algún filtrado adicional. 
● c) Analizar los tiempos de respuesta y patrones de conectividad 
Conexión al puerto 22: 
❖ Puerto 22 (SSH) está abierto en scanme.nmap.org, confirmado por la 
respuesta SYN-ACK. 
❖ El patrón observado (SYN → SYN-ACK → RST) indica un escaneo de puerto 
sin establecer conexión real. 
❖ No hubo pérdida de paquetes. 
❖ El tiempo de respuesta (RTT) es aceptable. 
Conexión al puerto 80: 
❖ El puerto 80 del host scanme.nmap.org está abierto, confirmado por la 
respuesta SYN-ACK. 
❖ El patrón observado (SYN → SYN-ACK → RST) indica un escaneo de puerto 
sin establecer conexión real. 
❖ No hubo pérdida de paquetes. 
❖ El tiempo de respuesta (RTT) es aceptable. 
Conexión al puerto 443: 
❖ Puerto 443 está cerrado o bloqueando conexiones en la página scanme.nmap.org,como lo indica el paquete (RST, ACK) enviado desde el servidor. 
❖ El host rechaza conexiones HTTPS, como lo demuestra el paquete RST enviado de vuelta. 
❖ El paquete llegó al servidor, pero fue rechazado, lo que sugiere que el puerto está cerrado o filtrado. 
❖ 100% de pérdida de paquetes según hping3. 
Conexión al puerto 21: 
❖ Puerto 21 (FTP) está cerrado o bloqueando conexiones en 
scanme.nmap.org,como lo indica el paquete (RST, ACK) enviado desde el servidor. 
❖ El host rechaza conexiones FTP, como lo demuestra el paquete RST enviado de vuelta. 
❖ El paquete llegó al servidor, pero fue rechazado, lo que sugiere que el puerto está cerrado o filtrado. 
❖ 100% de pérdida de paquetes según hping3. 
Recomendaciones 
1. Filtrado de paquetes no autorizados: Se recomienda configurar un firewall que bloquee paquetes ICMP innecesarios y conexiones entrantes a puertos no utilizados con el fin de evitar escaneos no autorizados. 
2. Uso obligatorio de cifrado: Asegurar que todo el tráfico web pase por HTTPS y evitar usar HTTP.  
3. Evitar protocolos obsoletos o inseguros como FTP (puerto 21), por protocolos más seguros como SFTP que se ejecuta sobre SSH (Puerto 22). 
4. Usar herramientas IDS/IPS como Snort o Suricata junto con Wireshark y alguna herramienta de detección y respuesta para monitorear continuamente la red y detectar y responder a comportamientos sospechosos. 
5. Revisar regularmente los puertos abiertos con escaneos de red internos y asegurarse de que los servicios asociados estén protegidos. 
6. Educar a los usuarios sobre los riesgos de sitios no seguros, ataques comunes y mejores prácticas. 
7. Implementar políticas de uso de contraseñas seguras y autenticación multifactor. 
Conclusiones 
Los hallazgos más importantes que se obtuvieron en este análisis, fueron los siguientes:  
Comportamiento de red ante escaneos: El uso de hping3 permitió verificar que 
muchos servidores (como google.com) ignoran o rechazan ciertos tipos de paquetes, lo 
que indica una correcta configuración de seguridad.  
Puertos abiertos, cerrados y filtrados: Durante el escaneo a puertos 21, 22, 80 y 443, 
se observó que algunos estaban abiertos (como el 80 y 22 ), mientras que otros estaban 
filtrados o cerrados(como el 21 y 443), lo cual es una buena práctica para identificar los 
puertos que están siendo utilizados y los que no son necesarios con el fin de reducir la 
superficie de ataque. 
Análisis del tráfico con Wireshark: El tráfico HTTPS (puerto 443) fue el más 
predominante, seguido de DNS y HTTP. Se identificó tráfico sin cifrar en algunas 
comunicaciones como HTTP y se logró diferenciar con el tráfico cifrado de HTTPS. 
Vulnerabilidades observadas: La exposición de tráfico sin cifrado (como HTTP) 
representa una vulnerabilidad importante. Esto puede ser aprovechado para ataques 
de sniffing, man-in-the-middle o inyección de contenido malicioso. 
Importancia del monitoreo continuo: El análisis demostró que muchas amenazas o 
configuraciones inseguras pueden pasar desapercibidas sin una herramienta de 
monitoreo y una política de auditoría periódica.
