Estoy intentando instalar una aplicación Rails corriendo sobre Debian detrás de
Nginx. La base de datos es MySQL.

Para eso, necesito instalar paquetes:

```bash
aptitude install -y vim git ruby ruby-dev build-essential \
  mysql-server libmysqlclient-dev \
  nodejs nodejs-legacy \
  nginx
```

Después viene la pregunta de si usar el ruby del sistema o instalar uno por
separado, para hacerse el loco, para hacerlo habría que usar rbenv o rvm o algo
así. El tema es que por un lado esto agrega complejidad, pero por otro, todos
los artículos que dan vueltas por Internet tienen esto en cuenta. Por el
momento, voy a intentar esquivar este paso.

Para configurar Nginx, la cuestión en debian es agregar un archivo en
`/etc/nginx/sites-available` para configurar el servidor. [1][]

```
server {
  listen 80;
  server_name mysite.com; # Replace this with your site's domain.

  root /var/www/application_name/current/public;

  try_files $uri/index.html
            $uri.html
            $uri
            @application;

  location @application {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-Forwarded_Proto $scheme;
    proxy_redirect off;
    # This passes requests to unicorn, as defined in /etc/nginx/nginx.conf
    proxy_pass http://unicorn;
    proxy_read_timeout 300s;
    proxy_send_timeout 300s;
  }

  # You can override error pages by redirecting the requests to a file in your
  # application's public folder, if you so desire:
  error_page 500 502 503 504 /500.html;
  location = /500.html {
    root /app/root/public;
  }
}
```

Después de eso, hay que iniciar el servidor de la aplicación como un demonio
del sistema. Ahí el tema se vuelve más aspero, porque ya no está claro que
corresponde a la configuración del servidor y qué a la aplicación, si uno
cambia de servidor de aplicación, que puede ser passenger, thin, unicorn, puma,
etc ¿debería tener que cambiar la configuración del servidor?

Ese punto no está muy claro aún en mi mente, por un lado, si uno quisiera dejar
eso abierto a que uno instala la aplicación que sea y anda, necesitaría un
script super-genérico que ejecute lo que sea que esté deployado y funcione bien
siempre, cuestión que es por cierto muy complicado.

Por otro lado si dejamos la cuestión de encender el servidor como un problema
de la configuración del servidor, necesitaríamos un script en '/etc/init.d' con:

```bash
#!/bin/sh

### BEGIN INIT INFO
# Provides:          aplicacion
# Required-Start:    $local_fs $remote_fs $network $syslog
# Required-Stop:     $local_fs $remote_fs $network $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the aplication web server
# Description:       starts the aplication web server
### END INIT INFO

if [ true != "$INIT_D_SCRIPT_SOURCED" ] ; then
    set "$0" "$@"; INIT_D_SCRIPT_SOURCED=true . /lib/init/init-d-script
fi

DESC="Name of the application"
DAEMON=/usr/sbin/daemonexecutablename
```

En el cual, el daemonexecutablename es un archivo que pertenece a nuestra
aplicación y funciona como nexo entre nuestra aplicación y nuestra
configuración del servidor. La dificultad radica en que ese
daemonexecutablename debe funcionar como init-d-script espera que las cosas
anden.

La cuestión sería tener un comando que pueda encargarse de correr un archivo
genérico, etc, etc, pero para eso hay que saber mucho, la opción simple parece
ser usar [thin][2] como server y una configfile para configurar la aplicación.

La otra opción, sería usar algo así como [foreman][3] para generar los init scripts
mediante export, pero la documentación no ayuda mucho.

En resumen, lo que se necesita, es crear un archivo de configuración por
aplicación que indique como se inicia la applicación, la inicie al inicio del
sistema y permita reiniciarla y detenerla. Para eso hay que entender sobre
pidfiles, logs, daemonizing y process. Tristemente, de todas esas cosas yo se
muy poco.

Así que me parece que voy a tener que pedir ayuda.

[1]: https://launchschool.com/blog/setting-up-your-production-server-with-nginx-and-unicorn "fuente"

[2]: https://github.com/macournoyer/thin "thin server"

[3]: http://ddollar.github.io/foreman/
