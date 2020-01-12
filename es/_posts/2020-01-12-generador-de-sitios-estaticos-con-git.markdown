---
title: Generador de sitios estáticos con git
tags: ideas
---

Los generadores de sitios estáticos, son una maravilla, lo que hacen es crear
un sitio web a partir de una serie de datos, que en general, están en simples
archivos en una carpeta, un comando crea a partir de estos, un sitio completo.

Una de las ventajas de usar una carpeta y archivos, además de poder editarlos
con muchísima facilidad, es que se pueden guardar en un repositorio de git. De
esta forma se hicieron populares gracias a las "github pages" que utilizan
"Jekyll".

Sin embargo, más allá de que casi siempre un sitio es generado a partir de un
repositorio, la información disponible en el repositorio es casi siempre
ignorada y se encuentra duplicada, ¿se podría pensar un generador de sitios
estáticos que utilice estos datos? De esta forma, el autor de un artículo es el
autor del commit, los editores también pueden quedar registrados de la misma
forma, se podrían generar también las versiones anteriores, las fechas de los
artículos se pueden extraer de los commits.

Siguiendo la misma lógica, se podrían crear redirecciones en caso de una nota
que cambió de nombre, se podrían firmar las notas utilizando `gpg` y aprovechar
muchas funciones que git ya tiene.

Hay algunos pocos avances hechos en esta linea, como un [plugin de jekyll][1]
para agregar metadatos desde git, pero me parece que esto se podría utilizar
bastante más.

 [1]: https://github.com/edsl/jekyll-post-revision
