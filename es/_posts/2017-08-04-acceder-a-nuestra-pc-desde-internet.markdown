---
---

Una de las cosas con las que siempre me topo al intentar jugar con Internet es
la de los intermediarios, el ISP no nos da una ip fija, así que es difícil
tener una dirección estable, y tampoco nos permite manejar el router, por lo
que una vez que sabemos nuestra ip, no podemos acceder a las computadoras que
están detrás.

Para solucionar el primer problema, una posibilidad que está muy buena es usar
[nsupdate.info][1] que te permite tener un dominio dinámico de manera gratuita.

El segundo paso es mantener la IP actualizada en nsupdate, para lo cual se
necesita el paquete [ddclient][2] que se encarga de avisar cuando cambia la ip,
configurarlo no es difícil, solo hay que tocar un poco el archivo de
configuración en `/etc/ddclient.conf`.

```
protocol=dyndns2
use=web, web=http://ipv4.nsupdate.info/myip
ssl=yes  # yes = use https for updates
server=ipv4.nsupdate.info
login=tuweb.nsupdate.info
password='secreto'
tuweb.nsupdate.info
```

Para el segundo problema, lo que necesitamos es informar a nuestro router a
donde dirigir los pedidos externos. Para hacerlo, en general hay que entrar a
la página de configuración del router e indicar el redireccionamiento de
puertos, pero hacerlo así es tedioso y en el caso de los routers que te manda
el proveedor puede ser imposible.

La solución es utilizar UPnP, que es un protocolo muy inseguro, que funciona en
casi todos los routers y permite controlarlo a distancia y configurar el _port
forwarding_ sin tocar casi nada. Para hacerlo necesitamos instalar `miniupnpc`
que provee el comando `upnpc` que permite hacer lo que decimos.

Después lo que necesitamos es un script que configure el redireccionamiento de
puertos cada vez que obtenemos una dirección ip local (o sea cuando la
computadora se conecta). Este script se escribiría más o menos así (no está
escrito aún):

```bash
#!/bin/sh
# /etc/dhcp/dhclient-exit-hooks.d/port_forwarding - exit hook for dhclient

[ -x /usr/bin/upnpc ] || exit 0

case $reason in
    BOUND | RENEW | REBIND)
        /usr/bin/logger -t dhclient $reason, adding port forwarding with upnpc
        /usr/bin/upnpc -a $new_ip_address 3000 80 tcp
        ;;
    *)
        ;;
esac
```

Esto debería hacer público el puerto 3000 de la computadora local en el puerto
80 de la ip externa. No estoy muy seguro de como se debería manejar el tema de
eliminar las reglas viejas y evitar hacer llamados de más, pero en general eso
debería bastar para la mayoría de los casos.

*[ISP]: Internet Service Provider (proveedor de internet)
*[UPnP]: Universal Plug and Play

[1]: https://www.nsupdate.info/
[2]: https://sourceforge.net/p/ddclient/wiki/Home/
