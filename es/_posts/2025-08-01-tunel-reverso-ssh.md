---
title: Túnel reverso SSH
---

Un túnel reverso permite acceder a un servidor que está detrás de un [NAT][],
como la gran mayoría, en el cual no podemos configurar re-direccionamiento de
puertos para pasar a través del mismo, o sea, que queremos acceder a un servidor
que tiene conexión a internet, pero que no está accesible desde internet.

Para hacerlo, lo que se hace es utilizar una conexión SSH (segura) desde la
computadora que queremos acceder a otra que funcione como proxy y establecer un
túnel. De ésta manera, una computadora externa puede conectarse al túnel en el
proxy para conectarse con la computadora en cuestión.

Las instrucciones están tomadas de varias fuentes, pero principalmente desde
[ésta nota en inglés][nota].

[nota]: https://www.xmodulo.com/access-linux-server-behind-nat-reverse-ssh-tunnel.html
## Instrucciones

Bueno, el primer paso es cambiar una configuración en el proxy, en particular en
el archivo `/etc/ssh/sshd_config` para permitir a los usuarios habilitar puertos
accesibles desde fuera. Sin este paso, la única PC capaz de acceder al servidor
sería nuestro proxy.

``` /etc/ssh/sshd_config
# Permite hacer port-forwardings accessibles desde afuera
GatewayPorts clientspecified
```

Después de hacer éste cambio hay que reiniciar el servicio `ssh` en el proxy.

Luego, una vez en el servidor, hay que configurar la conexión, para lo cual, yo
prefiero escribir todo lo posible en el archivo de configuración:

``` /root/.ssh/config
Host reverse-tunnel
  HostName proxy.example.com
  PasswordAuthentication no
  IdentityFile /root/.ssh/tunnel_rsa
  RequestTTY no
  RemoteForward 0.0.0.0:31540 localhost:22
  ExitOnForwardFailure yes
  ServerAliveInterval 60
  ServerAliveCountMax 3
```

Lo que estamos indicando es que para autenticarse use una llave privada en
particular y nunca utilice contraseña, que no intente iniciar una terminal, sólo
inicie el túnel reverso (_RemoteForward_), que si no logra establecer el túnel
falle y por último que falle si la conexión se pierde.

Un detalle importante es que "túnel" en inglés es "tunnel" (con dos 'n') y en
algunos casos prefiero dejar las configuraciones en un solo idioma.

La llave privada a la que hicimos referencia todavía no existe, pero se puede
crear muy fácilmente:

    $ ssh-keygen -C "tunnel@server" -f /root/.ssh/tunnel_rsa

Obviamente, no le vamos a poner ninguna contraseña, porque la idea es que este
proceso funcione sin intervención de ninguna persona.

Luego hay que permitir éste acceso en el proxy agregando la llave pública en el
archivo `authorized_keys`, pero en éste caso vamos a agregar opciones para
restringir al máximo el acceso que damos a dicha llave:

``` .ssh/authorized_keys
restrict,port-forwarding,permitlisten="0.0.0.0:31540" ssh-rsa AAAAB3blebleKE= tunnel@server
```

Con eso ya estaría todo lo necesario para establecer la conexión, así que
podemos probar el túnel, iniciándolo manualmente:

    $ ssh -v reverse-tunnel

Y después desde otra pc deberíamos poder hacer:

    $ ssh proxy.example.com -p 31540

Si eso funciona, podemos pasar al último paso, que es automatizar la
inicialización del túnel utilizando `autossh` que se va a encargar de iniciar la
conexión y reiniciarla si se cae.

``` /etc/rc.local
autossh -M 0 -fN reverse-tunnel
```

Los argumentos en ese llamado hacen lo siguiente:

- `-M 0`: des-habilita una funcionalidad de autossh que no necesitamos.
- `-f`: hace que el proceso se ejecute de fondo, sin molestar.
- `-N`: evita que autoSSH intente iniciar un TTY.


Eso debería permitir el acceder atravezando el NAT, espero que funcione,
cualquier cosa me avisan.
