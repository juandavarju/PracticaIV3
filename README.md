PrácticaIV3
===========

## Qué se va a hacer en esta práctica ##

Vamos a montar dos máquinas virtuales con distintas características y en la dos vamos a crear un servidor de correo con Postfix (Servidor de correo de software libre / código abierto, un programa
informático para el enrutamiento y envío de correo electrónico) . Después le haremos un benchmark para comprobar su rendimiento en ambas máquinas.


## Instalación del servidor de correo ##

Lo primero de todo es instalar apache:

<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/1.jpg />

Posteriormente instalamos Postfix:

<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/2.jpg />

Una vez instalado configuramos /etc/postfix/main.cf para hacer que funcione correctamente, añadiendo al final del documento:

<pre>

inet_protocols = ipv4
home_mailbox = Maildir/

</pre>

Después de reiniciamos Postfix para que se apliquen los cambios.

Ahora instalamos courier-pop y courier-imap para darle soporte pop e imap al servidor.

<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/3.jpg />

<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/4.jpg />

Instalamos mailx que nos permitirá enviar mensajes por la terminal

<pre>
apt-get install mailx
</pre>

Y por último Squirrel que es una aplicación web que nos permitirá gestionar nuestro correo.

<pre>
apt-get install squirrelmail
</pre>

Configuramos Squirrel con:

<pre>
squirrelmail-configure
</pre>

Para la configuración de Squirrel, la creación de usuarios y el envío de correo me he ayudado de esta página:
http://blogdeaitor.wordpress.com/2011/05/26/instalacion-y-configuracion-de-postfix-y-squirrel-en-linux/

## Cómo vamos a evaluar los datos ##

Vamos a usar Postal. Con él vamos a realizar las pruebas de estrés de nuestro servidor de correo. También para medir cuánto tarda un servidor en enviar mensajes usaremos smtp-source.

Para instalar smtp-source necesitamos instalar mailutils
<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/5.jpg />

Postal incluye Postfix al instalarlo.

## Servidores ##

Máquina A.
Máquina virtual.
Sistema Operativo: Ubuntu server 12.04
Procesador: 2 núcleos.
RAM: 1024MB.

Máquina B
Máquina virtual
Sistema Operativo: Ubuntu server 12.04
Procesador: 1 núcleo
RAM: 4096MB

## Pruebas realizadas ##

Prueba de estrés:
<br>
  Con varias hebras:
  <br>
    Gran volumen de mensajes. Mensajes de gran tamaño
    
<pre>
    postal -t 4 -m 5120 -r 1000 localhost user-list
</pre>

Gran volumen de mensajes. Mensajes de tamaño pequeño

<pre>
    postal -t 4 -m 512 -r 1000 localhost user-list
</pre>

  Con una hebra:
  <br>
Gran volumen de mensajes. Mensajes de gran tamaño
<pre>

postal -t 1 -m 5120 -r 1000 localhost user-list
</pre>

Gran volumen de mensajes. Mensajes de pequeño tamaño
<pre>

postal -t 1 -m 512 -r 1000 localhost user-list
</pre>

t indica el número de hebras, m el tamaño del mensaje en KB, r el número de mensajes a enviar por minuto y user-list el archivo donde se encuentran a que direcciones vamos a enviar el correo.
<br>
<br>
<br>
<br>
Pruebas de rendimiento/tiempo
<br>
Con una hebra:
<br>
    Gran volumen de mensajes. Mensajes de gran tamaño
<pre>
time smtp-source -c -l 5120 -t juanda@localhost -s 1 -m 1000 localhost
</pre>
Gran volumen de mensajes. Mensajes de pequeño tamaño
<pre>
time smtp-source -c -l 512 -t juanda@localhost -s 1 -m 1000 localhost
</pre>
Con varias hebras:
  <br>
Gran volumen de mensajes. Mensajes de gran tamaño
  <pre>
  time smtp-source -c -l 5120 -t juanda@localhost -s 4 -m 1000 localhost
  </pre>
Gran volumen de mensajes. Mensajes de tamaño pequeño
  <pre>
  time smtp-source -c -l 512 -t juanda@localhost -s 4 -m 1000 localhost
  </pre>
  
El comando c hace que se muestre el contador de mensajes, l el tamaño del mensaje en KB, t la dirección a la que envía los mensajes, s el numero de hebras y m el numero de mensajes a enviar.

## Resultados ##
Pruebas de estrés en máquina A
<br>
<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/6.jpg />
<br>
Pruebas de estrés en máquina B
<br>
<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/7.jpg />
<br>
Prueba de rendimiento/tiempo en máquina A
<br>
Gran volumen de mensajes. Mensajes de gran tamaño
<br>
<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/8.jpg />
<br>
Gran volumen de mensajes. Mensajes de tamaño pequeño
<br>
<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/9.jpg />
<br>
Prueba de rendimiento/tiempo en máquina A
<br>
Gran volumen de mensajes. Mensajes de gran tamaño
<br>
<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/10.jpg />
<br>
Gran volumen de mensajes. Mensajes de tamaño pequeño
<br>
<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/11.jpg />
<br>

## Conclusión ##
Como podemos ver en los datos la máquina A tiene un rendimiento superior a la máquina B. Vemos que lo que realmente hace cuello de botella en el servidor es el procesador con lo que dependiendo del flujo de tráfico que vaya a tener será la opción más a tener en cuenta a la hora de realizar una ampliación.
