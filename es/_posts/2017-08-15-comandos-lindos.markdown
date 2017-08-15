---
---

Cada tanto encuentro que uno de esos comandos que siempre me cuestan mucho
tiene una alternativa, que de paso es más potente. Uno de los primeros comandos
que aprendí en GNU/Linux es `ifconfig`, que al parecer hace un montón de cosas,
pero yo siempre lo usé para saber mi dirección ip local.

Hoy encontré una alternativa a ese comando, el comando `ip` que es parte de
iproute2. Al parecer la idea es que iproute reemplace a los viejos comandos
pero como la configuración en muchos archivos ya está escrita la migración es
lenta. Por cierto, iproute se comenzó a desarrollar en 2004, por lo tanto está
bastante maduro.

Con este paquete, para saber la dirección ip de la máquina, el comando que hay
que ejecutar es:

```bash
$ ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 4b:cc:6a:45:16:80 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.29/24 brd 192.168.0.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::4ecc:6aaf:fe45:1680/64 scope link 
       valid_lft forever preferred_lft forever
```

Como se ve el nombre del comando es mucho más memorable y la salida es bastante
más legible que la de ifconfig.

Además de esa utilidad, `ip` permite listar y controlar todo el sistema de
routeo de la pc, también está `ss` que manipula sockets y `tc` que permite
implementar _control de tráfico_.
