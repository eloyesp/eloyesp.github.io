---
title: "Magia con bash: getopts"
---

Hoy modifiqué un script de bash para aceptar opciones, lo que necesitaba no era
muy complejo, pero a veces en bash las cosas cuestan un poco más de lo que
deberían. La cuestión es ver qué es y por qué está ahí, cada cosa, así que acá
está la versión supercomentada.

``` bash
#!/bin/bash

set -e # explota en caso de error (en vez de seguir como si nada)

arguments="-Ai" # argumentos por defecto

# Acá empieza la diversión, por un lado hay un bucle while, que se va a
# ejecutar una vez por cada opción que machea (y también en caso de opciones
# que no machean.
# getopts devuelve true (0) cada vez que encuentra una opción y la guarda en la
# variable `option`
while getopts "hben:0rx" option
do
  # Por cada opción, nosotros tenemos un case que hace algo y termina con `;;`
  case "$option" in
  x) set -x;;
  b) arguments="$locate_arguments -b";;
  e) arguments="$locate_arguments -e";;
  n) arguments="$locate_arguments -n $OPTARG";;
  0) arguments="$locate_arguments -0";;
  r) arguments="$locate_arguments -r";;
  # Si nos encontramos con una opción que no machea (o con h), entonces
  # mostramos la ayuda *y salimos*
  *) cat <<HELP
Uso: ${0##*/} [OPCIÓN]... [PATRÓN]...
Buscar archivos en el disco y en el backup
  -h, --help       imprimir esta ayuda
  -b, --basename   coincidir solo el nombre base de los nombres de ruta
  -e, --existing   chequear que los archivos Existen
  -n LÍMITE        limitar salida (o recuentos) a LÍMITE entradas
  -0, --null       separar entradas con NUL en la salida
  -r, --regex      los patrones son regexps extendidos
  -x               activar xtrace
HELP
     exit 0;;
  esac
done

# y por último cerramos, tanto el case (con esac) como el do (con done).

# El problema es que todos esos argumentos que leímos quedaron ahí, la
# siguiente linea se encarga (ni idea cómo), de eso.
shift $((OPTIND-1))

locate $arguments "$@"
```

La verdad que quedó muy lindo y no tan largo, la única mentira que tiene es las
opciones largas en la ayuda (`--basename`), que en realidad no andan, pero me
importa poco.
