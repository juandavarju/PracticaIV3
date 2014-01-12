PracticaIV3
===========

## Que se va ha hacer en esta práctica ##

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

Para la configuración de Squirrel, la creación de usuarios y el envío de correo me he ayudado en esta página:
http://blogdeaitor.wordpress.com/2011/05/26/instalacion-y-configuracion-de-postfix-y-squirrel-en-linux/

## Como vamos a evaluar los datos ##

Vamos a usar Postal. Con el vamos a realizar las pruebas de estrés de nuestro servidor de correo. También para medir cuanto tarda un servidor en enviar mensajes usaremos smtp-source.

Para instalar smtp-source necesitamos instalar mailutils
<img src=https://dl.dropboxusercontent.com/u/71428812/ivp3/5.jpg />

Postal lo incluye Postfix al instalarlo.

## Servidores ##

Máquina A

Sistema Operativo: Ubuntu server 12.04
Procesador: 2 nucleo
RAM: 1024MB

Máquina B

Sistema Operativo: Ubuntu server 12.04
Procesador: 1 nucleo
RAM: 2048MB


