---
title: Tunel reverso SSH
---

La cuestión es que por una causa molesta tengo que lograr acceder por `ssh` a
una pc que es inaccesible, por ejemplo, porque está detrás de un NAT, así
empezó la cosa, pero por suerte no es al primero al que le pasa, así que hay
bastante ayuda en Internet.

El problema es que esa mucha ayuda toca muchos detalles, es un problema viejo y
hay soluciones variadas, dispersas, así que para futuras referencias voy
intentar dejar una guía más cerradita.

## Sobre el problema y la solución

La idea es poder acceder a una pc que está detrás de un NAT, que puede
conectarse con otra, pero las otras computadoras no pueden conectarse con ella.
Entonces, la solución es establecer un canal de esa computadora a otra que si
sea accesible y utilizar ese canal para conectarse a la computadora. (ese canal
de comunicación es el "_tunel_").

Para que esto se entienda mejor, vamos a ponerle nombre a los personajes: la
computadora a la que quiero acceder pero está encerrada es "V", la
computadora a la que si se puede acceder y que funcionará como _proxy_ es
"C" y la computadora desde dónde me quiero conectar es "R". Con esos
nombres lo que hago es establecer una conexión SSH desde V hacia C,
estableciendo un tunel reverso, dejo esa conexión establecida, luego me conecto
desde R a un puerto particular en C que me permite a través del
tunel hablar con V.

## Los detalles

Para que esto funcione primero necesitamos habilitar algunas funciones
especiales en `sshd` en Carlos, estas opciones nos van a permitir abrir el
tunel para que sea accesible desde Carlos y nos va a asegurar que la conexión
no se caiga.

``` sshd
# /etc/ssh/sshd_config (en el "proxy": C)

GatewayPorts clientspecified
ClientAliveInterval 20
ClientAliveCountMax 10
```

Y luego vamos a necesitar reiniciar el servicio sshd (`sudo service sshd
restart`)

El segundo paso es habilitar una llave ssh para abrir este puerto, en V creamos
una clave usando `ssh-keygen` sin contraseña y la dejamos por ejemplo en
`~/.ssh/tunnel_rsa`.

Como la llave ssh que creamos no tiene contraseña, vamos a darle permisos muy
limitados en C para lo cual vamos a utilizar una linea de autorización muy
específica:

``` authorized_keys
# ~/.ssh/authorized_keys (C)

restrict,port-forwarding,permitlisten="0.0.0.0:2232" ssh-rsa AAAAB3NzaC....A2KE= tunnel@V
```

En este caso, lo que hacemos es primero quitarle todos los permisos con
`restrict`, luego habilitamos el `port-forwarding` y lo restringimos al puerto
que queremos. En esta linea va a ser necesario copiar la llave pública que
creamos en el paso anterior.

Ahora viene la parte de establecer la conexión, y la mayoría de las opciones
las vamos a escribir en el archivo config, dónde va a ser más legible.

``` config
# .ssh/config (V)

Host tunnel-c
  HostName c.example.org
  Port 2222
  RemoteForward 0.0.0.0:2232 localhost:22
  ExitOnForwardFailure yes
  IdentitiesOnly yes
  IdentityFile ~/.ssh/tunnel_rsa
  ServerAliveCountMax 10
  ServerAliveInterval 20
```

En los casos de HostName y Port va a ser necesario ajustarse a los detalles de
la conexión de V con C (que cómo premisa estaba funcionando). El `RemoteForward`
es el más difícil de leer, pero lo que establece es un tunel entre C:2232 y
V:22. Las opciones relativas a `Identity` permiten que usen la llave que
generamos (sin contraseña) y por último otras opciones para asegurarse que la
conexión no se pierde.

El paso final es establecer la conexión y evitar que esta se caiga o que si se
cae vuelva a funcionar, para lo cual vamos a usar `autossh` que está hecho para
eso:

    autossh -f -M 0 -S none -N tunnel-c

Las opciones ahí son `-f` para correr como proceso de fondo, `-M 0` para dejar
que `ServerAlive` y `ClientAlive` sean los encargados de verificar que la
conexión está funcionando (si no autossh crea otras redirecciones, pero ya no
debería ser necesario), `-S none` va a evitar que esta sesión SSH utilice una
conexión Maestra y finalmente `-N` indica que no queremos una sesión
interactiva (que no va a funcionar porque es una credencial restringida).

Finalmente, desde R, podemos establecer la conexión con V utilizando la
siguiente configuración:


``` config
# .ssh/config (R)

Host V-with-tunel
  HostName c.example.org     # el hostname de C
  Port 2232                  # el puerto dónde instalamos el tunel
```
Obviamente, vamos a necesitar credenciales autorizadas en V para acceder desde
R, eso no cambia.

Espero que sirva, cualquier comentario no duden en contactarme.
