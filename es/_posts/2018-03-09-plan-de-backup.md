---
---

Desde hace un tiempo estoy intentando pensar como organizar los documentos en
la pc, que carpetas usar y como categorizar. El otro día, pensando como
organizar el backup se me ocurrió una categorización que tiene sentido. Mi
interpretación es que al pensar como hacer backup de los datos, uno piensa
cómo se usan esos datos, con que frecuencia se acceden y modifican y que tan
preciados son.

## documentos (de trabajo)

La primera carpeta que se me ocurre pensar es muy genérica, pero sólo por
falta de una palabra para identificarlo. Los documentos, son archivos
principalmente de texto, de uso cotidiano y modificación frecuente,
relacionados principalmente con el trabajo (ya sea rentado o no).

Estos archivos, en general son pequeños, en este momento no requieren más de
500Mb y se modifican de forma frecuente. En general, los documentos tienen un
tiempo de utilidad limitado y pocos se vuelven realmente de consulta. El
problema es que entre estos se suele juntar mucha mugre, que impide que los
documentos más importantes sean consultados.

Para evitar eso, la idea es hacer un script de backup que realice un backup de
frecuente y se almacenen distintas versiones por ejemplo: 3 diario, 2 semanal,
5 mensual, 1 anual. En realidad esto no requiere mucho espacio (porque la
mayoría de los archivos no cambian), y el mismo se puede realizar utilizando
rsync y enlaces duros.

De cualquier manera, por seguridad estaría bueno también realizar un backup
mensual de estos archivos en una computadora distinta.

## fotos (recuerdos)

En realidad, el nombre debería ser recuerdos, es un conjunto de archivos de
acceso frecuente que no se modifican casi nunca. El tema con estos es que son
considerados muy valiosos, por lo que esta bueno asegurarse que queden
guardados.

En este caso, la idea sería realizar un backup mensual en otra pc (si fuera
posible mantener dos copias). El problema, es que en este caso los archivos
son más grandes, el almacenamiento requerido al día de hoy son 6Gb

## videos (películas)

Estos archivos en general son muy grandes y en el peor de los casos se pueden
recuperar, la idea es mantener una copia simple con rsync.

## proyectos (git)

Como guardar los proyectos de código es otra cosa que no se muy bien como
resolver, la cuestión es que los proyectos están en repositorios de git, por
lo que son muchos archivos que no tienen sentido y convendría guardar los
directorios, este caso no lo tengo muy bien pensado, pero al parecer el
comando git-bundle podría ser la solución.

En cualquier caso el resultado sería guardar todos esos archivos con rsync en
alguna otra pc y el almacenamiento requerido no es mucho, por ahora 1Gb.

## El script

Hace un rato comenté que se pueden realizar muchos backups de lo mismo con
pocas diferencias sin volver a reescribir todo usando rsync, el script que hay
que usar sigue las siguientes lineas:


~~~ bash
#!/bin/bash

set -e

doc_src=/home/eloyesp/documentos

cd /media/backup/documentos

doc_dest=daily-$(date +%F)
doc_last=$(ls | grep daily | sort | tail -1)

if [ -d "$doc_dest" ]
then
  echo "backup already done"
  exit 1
fi

keep_newers () {
  PATTERN=$1
  COUNT=$2

  ls |
  grep daily |
  sort |
  head -n -$COUNT |
  xargs --no-run-if-empty rm -rf
}

# copia el último backup haciendo enlaces duros (no ocupa espacio)
[ -d "$doc_last" ] && cp -al $doc_last $doc_dest

rsync --archive --delete $doc_src/ $doc_dest

keep_newers daily 7
~~~

El secreto está en que si rsync no encuentra ningún cambio deja el archivo
como está, pero si se realizan cambios rompe el enlace con el archivo anterior
y escribe uno nuevo.

Queda como tarea escribir estos comandos de una forma más clara para escribir
los scripts de backup.
