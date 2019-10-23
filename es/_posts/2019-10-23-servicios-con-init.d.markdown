---
---

Hoy estuve agregando servicios utilizando init-d y logré que funcionen más o
menos bien, la principal dificultad es que en internet hay mucha info
contradictoria e incompleta, en gran medida porque el sistema tiene mucha
historia y fue cambiando.

A la final, se pudo hacer escribiendo un archivo, en este caso
`/etc/init.d/faircoind` porque lo que quería era correr un nodo completo de
faircoin, que quedó así: (tiene que ser ejecutable)

~~~ bash
#!/usr/bin/env /lib/init/init-d-script
### BEGIN INIT INFO
# Provides:          faircoind
# Required-Start:    $local_fs $networking $syslog $time
# Required-Stop:     $local_fs $networking $syslog $time
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Description:       Faircoin full node
### END INIT INFO
DAEMON=faircoind
PIDFILE=/home/faircoin/.faircoin2/faircoind.pid
START_ARGS="--chuid faircoin"
DAEMON_ARGS="-daemon"
~~~

Para escribir eso, son necesarias algunas fuentes, entre las cuales están:

La página del manual sobre `init-d-script` y el archivo (es un script de bash),
porque muchas de las variables no están documentadas. En particular,
`START_ARGS` me pareció bastante importante. Y también la página sobre
`start-stop-daemon` que es el que finalmente ejecuta el demonio.

Para probar si anda, el comando sería `VERBOSE=true service faircoind status` y
una vez que vemos que anda, deberíamos crear los enlaces para que se inicie y
se cierre junto con el sistema (crear los enlaces en otros directorios) con el
comando `update-rc.d faircoind defaults`.
