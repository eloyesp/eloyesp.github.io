---
title: Instalar OBS multi RTMP en EterTICs Yetapa
---

Ayer vino mi hermano con el desafío de instalar un plugin de OBS que estaba
complicado, al final lo logramos compilación mediante, los detalles a
continuación.

[EterTICs][etertics] es una distro GNU/Linux para radios libres, que en este caso está
basado de [Devuan][] 4 (Chimaera) que está basado en Debian 11 (Bullseye). Por lo
tanto, tiene versiones estables de todo, entre ello del [OBS][].

Por esa misma causa, a veces instalar un plugin puede ser un poco más
desafiante, porque hay que encontrar la versión que funciona con la versión de
OBS correspondiente, en este caso 27.

En este caso, eso se complica un poco más, porque [éste plugin][plugin] está
desarrollado y documentado principalmente en japones y diseñado principalmente
para Windows, por lo que hacerlo andar es más desafiante aún.

Al final, hacerlo andar no fue taaaan difícil, pero quizás encontrar estas
notas pueda ayudar a otro a recorrer éste mismo camino con menos tropiezos.

Primero, buscamos la última versión con soporte para OBS 27, buscamos en los
[releases de github][releases] y encontramos que la versión que buscábamos era
la `0.2.8`. Pero en el release no había un binario para Linux listo para
descargar, así que lo que hicimos fue bajar el código fuente.

Mi forma de hacerlo, fue clonar el repositorio de Git:

    git clone https://github.com/sorayuki/obs-multi-rtmp.git

Luego para ir a la versión correcta busqué entre los "_tags_" el tag
correspondiente, pero al final el que funcionó fue el `0.2.8.1` (que no tiene
release asociado).

    git checkout 0.2.8.1

Luego viene la parte de compilar, para lo cual necesitamos instalar algunos
paquetes:

    sudo aptitude install build-essential cmake \
                          qtbase5-dev qtdeclarative5-dev libobs-dev

Luego, al menos en ésta versión, hay un `script` llamado `build_linux.sh` que
hace el resto del trabajo:

    bash build_linux.sh

Eso nos va a dar un archivo `obs-multi-rtmp.so` en la carpeta `build` y los
`locales` en la carpeta `dist/usr` que son los archivos que necesitamos para
sacarlo andando.

[etertics]: https://gnuetertics.org/
[OBS]: https://obsproject.com/es
[Devuan]: https://www.devuan.org/
[plugin]: https://sorayuki.github.io/obs-multi-rtmp/
[releases]: https://github.com/sorayuki/obs-multi-rtmp/releases
