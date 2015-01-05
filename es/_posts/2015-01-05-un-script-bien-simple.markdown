---
title: Un script bien simple
layout: post

---

Acabo de escribir un script de shell. Hace una cosa muy simple, pero de otra
manera la hubiera tenido que hacer yo cada vez...

Ciertamente me tomo un rato, algo menos de una hora, y solo hace una cosa, me
pregunta el título de mi nota de blog, crea el archivo con el nombre correcto y
después me abre el editor con la nota lista para ser escrita.

El código es este:

~~~bash
#!/bin/bash

set -e

new() {
  echo -n "Nombre de la nota: "
  read TITULO
  DATE=`date +%Y-%m-%d`
  FILENAME="es/_posts/$DATE-${TITULO// /-}.markdown"
  echo "---
title: $TITULO
layout: post

---

" >> $FILENAME
  vi $FILENAME
}

case "$1" in
  new) new;;
esac
~~~

El código es muy simple pero tiene como ventaja agregada que para aprender
cómo hacerlo, tuve que aprender bash, así que eso que aprendí me va a servir
por largo rato (bash va a seguir ahí) y me lo voy a seguir cruzando un tiempo
más y lo mismo pasa con `date`.

Si un día de estos yo dejo de usar Jekyll para tener mi blog puedo adaptar mi
script ya que es bastante chico y simple, por lo que mi flujo de trabajo se
hace más independiente de la herramienta de turno y tiene más que ver con lo
que yo hago.

En resumen, este script, así de pequeño, me parece que es para mi un gran paso,
con el objetivo de hacer que mis aprendizajes no sean tan efímeros y caigan en
la nada con la siguiente versión de gnome/blog/firefox o la herramienta gigante
de turno.

---

## Post-data

Por otro lado, ahora puedo contarles que antes de empezar este script, estaba
por escribir una nota sobre systemd, comentando lo malo que me parecía por ser
uno de esos avances para atrás, nota que posiblemente núnca sea escrita, pero
ahora que lo veo desde este lado, me parece que este script, es un aporte mucho
más interesante en ese sentido. Hoy si me siento un hacker. :)
