---
title: Impresoras CUPS y LSB
---

La gente de GNU/Linux a veces hace cosas molestas, como por ejemplo, cambiar
interfaces, haciendo que cosas que antes andaban (aunque eran feas), ahora no
andan. Este es el caso de los drivers de mi impresora Epson, que son feos y
privativos, pero los necesito y de pronto no anda, porque se dió de baja algo
que se llama LSB.

## Qué es LSB

Al parecer LSB era una biblioteca que funcionaba como interfase para hacer
andar drivers privativos en el sistema, en algún momento lo dieron _deprecaron_
y crearon algo así como un "paquete de transición" para poder usar la mayoría
de los drivers que dependen de este sistema.

## Qué se rompió (y como arreglarlo)

Bueno, la primera evidencia del problema, es que cuando uno finalmente logra
encontrar el paquete `deb` (extensión de los paquetes de Debian y derivados)
correspondiente a la impresora este falla porque falta una dependencia, que no
está disponible.

Buscando y rebuscando, encuentro una guía que explica lo que se supone que uno
debería saber de forma mágica y que además es muy poco intuitivo: uno debería
descargar el paquete `rpm` (los paquetes de Fedora y RedHat), para después
generar un paquete `dev` usando `alien` (que es un paquete que no sabía que
existía).

    sudo aptitude install alien
    sudo alien -c epson-inkjet-printer-201207w-1.0.0-1lsb3.2.x86_64.rpm

De pronto, no se bien como tenemos ahora un paquete `deb` que si podemos
instalar, que no depende de `lsb`, pero como quien no quiere la cosa, depende
de `alien`.

Ok, ahora de pronto podemos instalar la impresora, pero por desgracia, no
imprime y saber por qué es un camino nuevamente muy difícil, porque los
mensajes de error son erróneos.

El primer mensaje de error que nos encontramos en la interfase web de CUPS es
"_filter failed_", cuestión que nos quiere decir que hay un filtro que falló,
pero no nos dice ni cuál, ni por qué y peor aún... no hay casi información de
algo llamado filtros vinculado a CUPS.

## Filtros en CUPS

Al parecer, cuando uno manda un documento a imprimir, CUPS aplica sobre este
una serie de filtros que transforman el archivo en algo aceptable por la
impresora, el archivo `pps` de la impresora indica qué filtros necesita
ejecutar, en mi caso:

``` ppd
# /etc/cups/ppd/EPSON_L555_Series.ppd
*cupsFilter:    "application/vnd.cups-raster 0 /opt/epson-inkjet-printer-201207w/cups/lib/filter/epson_inkjet_printer_filter"
```

## Errores en los logs

Pero para saber qué falla con el filtro es necesario leer los logs, pero antes
hay que configurar CUPS para que genere dichos logs, así que hay que modificar:

``` conf
# /etc/cups/cupsd.conf
# Log general information in error_log - change "warn" to "debug"
# for troubleshooting...
LogLevel debug   
```

Eso nos va a permitir leer, luego de reiniciar cups y mandar nuevamente algo a
imprimir, entre muchos mensajes confusos otro mensaje de error, más confuso
aún... 

``` log
# /var/log/cups/error_log
D [03/Oct/2023:11:38:59 -0300] [Job 23] PID 14239 (/opt/epson-inkjet-printer-201207w/cups/lib/filter/epson_inkjet_printer_filter) stopped with status 102 (No such file or directory)
```

Vemos que parece que algo falla porque el archivo no existe, pero el archivo
está ahí, lo que falla es el mensaje de error, que no está indicando qué
archivo falta y quién lo necesita. Lo que pasa, lo que pasa es que el archivo
ejecutable `epson_inkjet_printer_filter` es un archivo `ELF` vinculado a `LSB`
que al parecer necesita un intérprete y ese intérprete es el que _falta_.

    # file /opt/epson-inkjet-printer-201207w/cups/lib/filter/epson_inkjet_printer_filter
    /opt/epson-inkjet-printer-201207w/cups/lib/filter/epson_inkjet_printer_filter: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-lsb-x86-64.so.3, for GNU/Linux 2.6.15, BuildID[sha1]=639e85f237ed13ade44f586e21ff61bdc1498a9c, not stripped

El interprete que nos falta es `/lib64/ld-lsb-x86-64.so.3` que es parte del dichoso paquete `lsb` que ahora no está. Por suerte para nosotros, el archivo si está, pero no está con el nombre que nuestro privativo driver lo está buscando y le podemos dar el gusto sin mucho trabajo utilizando un enlace simbólico:

    sudo ln -s /lib64/ld-linux-x86-64.so.2 /lib64/ld-lsb-x86-64.so.3

Y ahora finalmente, la impresora está andando.
