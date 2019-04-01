---
title: ElectrumFair
---

Hoy intenté finalmente enfrentar la tarea de instalar un servidor de
[Electrum][], lo hice principalmente porque necesitaba entender un poco como
funciona todo eso y no encontré otra forma más que poniendo manos a la obra.

Hacerlo fue bastante un viaje, así que voy a intentar contar un poco cuales
fueron mis pasos, que no van a ser los de todo el mundo, pero quizás ayuden a
más de uno, especialmente a mi.

Lo hice usando faircoin, por varias causas, pero creo que la principal, es que
la cadena es más liviana y por lo tanto puedo tratar sin bajar media internet.

## Paso uno, el dominio

Por alguna causa empecé por este tema, no tengo un servidor en serio, tengo la
conexión de mi casa y la de la oficina, así que tengo una IP dinámica y
necesito entonces un DNS dinámico. El que vengo usando es https://nsupdate.info
que me permite conectar https://poderosa.nsupdate.info, donde también está
alojado este blog. El problema que tengo ahí es que [no permiten manejar los
registros de DNS][1] y por lo tanto no me permiten tener varios servicios en la
misma ip (utilizando varios nombres de dominio).

Entonces intenté probar con [duckdns][] que tiene un servicio similar, pero es un
poco  más fácil de usar. Ahí configuré el dominio http://electrumfair.duckdns.org
y idealmente lo mantengo actualizado con un script que escribí:

~~~ bash
#!/bin/sh
# /etc/dhcp/dhclient-exit-hooks.d/electrumfair
# update duckdns domains
# port forward
(
    export DOMAINS=electrumfair
    export TOKEN=jb21o4i2-1354-4518-4497-añsdklfnañsc

    case $reason in
        BOUND | RENEW | REBIND)
            /usr/bin/logger -t electrumfair $reason, updating IP address with duckdns
            /usr/bin/curl \
              "https://www.duckdns.org/update?domains=$DOMAINS&token=$TOKEN" |
              /usr/bin/logger -t electrumfair
            /usr/bin/logger -t electrumfair making port forwarding
            /usr/bin/upnpc -e electrumx-fair -r 51812 tcp
            ;;
        *)
            ;;
    esac
)
~~~

Este archivo se ejecuta al recibir cualquier cambio por dhcp y si lo que pasó
es que hay una nueva dirección ip: actualiza la ip externa en duckdns y abre
los puertos utilizando upnp.

Por cierto, no estoy seguro si es necesario abrir el puerto 40405.

Por otro lado, este script usa `curl` y `miniupnpc`.

## Nodo FairCoin completo

Bueno, en este punto yo tenía dos fuentes de datos principales, por un lado la
[documentación que hay en electrum][2] y por otro la [documentación de
ElectrumX][3] que al parecer es el servidor de Electrum.

Ahí empece a ver que Electrum tiene una fuerte dependencia de estos nodos, a
los cuales confía varios secretos, no claves privadas, pero si la totalidad de
las claves públicas, con los cuales, puede vincular la ip de una persona con
todos sus fondos. En este sentido, funciona muy distinto de un nodo completo,
que al bajar toda la cadena, nadie puede saber que parte le interesa y cual no.

Según el HOWTO, lo que yo necesito como primer paso es un nodo completo, que ya
tengo, pero con algunas configuraciones, que al parecer se escriben de la
siguiente forma:

~~~
# ~/.faircoin2/faircoin.conf
txindex=1
rpcuser=fnañduiofnadhf0a8s7dhfnqkjrnlkajhdfblaks
rpcpassword=pio23opiunhapinfaopiufasd8f7asdf878ha08d
~~~

Una vez que está ese archivo se puede ejecutar `faircoind -reindex` (descargado
de https://download.faircoin.world/) y dejarlo corriendo.

## ElectrumX

Bueno, ejecutar electrumx fue bastante complicado, me ayudo mucho usar
[direnv][] con la siguiente configuración en un directorio que cree para la
ocasión:

~~~ bash
# .envrc 
layout python3

export COIN=FairCoin
export DB_DIRECTORY=$PWD/electrum_database
export DAEMON_URL="user:password@localhost"
export HOST="0.0.0.0"
export TCP_PORT=0
export SSL_PORT=51812
export REPORT_HOST="electrumfair.duckdns.org"

export SSL_CERTFILE="$PWD/server.crt"
export SSL_KEYFILE="$PWD/server.key"
~~~

Estas variables de entorno indican a electrumx cómo funcionar, la moneda es
FairCoin, la base de datos está en ese directorio (es otra base de datos,
especial para electrum, es como un índice super rápido de la cadena de bloques)

También está la información de como conectarse con `faircoind` (el user y
password deberían corresponder a los que indicamos en el archivo anterior), el
host indica que sea público y finalmente el REPORT_HOST es el nombre de host
público por el cual podemos ser encontrados.

Por último está el certificado SSL, que es una llave auto-firmada que se crea
con el comando:

    openssl req -new -newkey rsa:2048 -sha1 -days 365 -nodes -x509 -keyout server.key -out server.crt

Tengo entendido que hay mejores formas de crear esta llave, de forma que esté
firmada por una clave de confianza, pero por ahora eso funciona. La otra opción
sería configurar [Let's Encrypt][], pero eso es otro viaje.

## Siguientes pasos

Con estos pasos electrumfair quedó funcionando, excepto por dos cuestiones, por
un lado los nodos principales (que estan hardcodeados en electrumfair) no
comentan sobre mi nodo (por lo que nadie excepto yo estoy conectado).

Por otro lado, todo esto fue para poder hacer llamados rpc a electrum, y aún no
pude hacerlo, pero bueno, un paso a la vez.

 [1]: https://github.com/nsupdate-info/nsupdate.info/issues/105
 [2]: https://electrum.readthedocs.io/en/latest/merchant.html
 [3]: https://electrumx.readthedocs.io/en/latest/HOWTO.html
 [electrum]: https://electrum.org/
 [direnv]: https://direnv.net/
 [duckdns]: https://www.duckdns.org/
 [Let's Encrypt]: https://letsencrypt.org/
