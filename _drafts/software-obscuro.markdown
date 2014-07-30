---
layout: post
title: Software obscuro
---

Cada ambiente es un mundo y ese mundo se piensa con palabras. Cuando uno es nuevo y recien llegado hay un montón de cosas que parecen iguales (como cuando empieza a escuchar música electrónica), hasta que uno acostumbra el oído y empieza a encontrar sutiles diferencias, y uno termina descubriendo que esas dos cosas que parecían iguales no tenían nada que ver una con otra.

El mundo del software no es la excepción, y cosas que al principio parecen casi iguales, (como ubuntu y debian) terminan siendo abismalmente distintas. Así si nos preguntan hoy cuál es la diferencia entre Java y Ruby, muchos de los que estan acá podrían hablar un rato largo.

Para marcar la diferencia, comienzan a utilizarse conceptos cada vez más específicos, para identificar esas diferencias, que para los recién llegados parecen iguales, y esas palabras las hacen visibles.

Así me choqué con la cuestión de libre y abierto, que mirado a primera vista parecen iguales, pero hay más que sutiles diferencias. El software libre no solo te deja ver que está pasando, te deja compartirlo, modificarlo, trabajar a partir de ese software.

Sin embargo en las últimas charlas nos encontramos con un hueco, yo me veía obligado a decir ese software no es "tan libre" y Olvap no paraba de decirme que no estaba hablando de software libre, que la idea de libre no habla de cómo es el código o la aplicación. *?*

## pausa para una anecdota ##

Ahora hacemos una pausa y planteamos un caso extremo y a medias cierto **es necesario decir esto último?**, nos llaman para revisar una aplicación abierta y libre que había comprado un cliente, revisamos el codigo en PHP y nos encontramos con codigo generado por otro software. Sí, abierto, libre y generado por otro software, a partir de otro código fuente, incomprensible, con errores y feo.

Ese software no es "tan libre", porque su libertad es una fachada, una cuestión de licencias.

## palabras ##

Entonces es que aparece una sutil diferencia que vale tener su propia palabra, porque realmente software libre es una cuestión de licencias y no de software; y hay diferencias en el cómo *(se puede decir a qué hace referencia ese cómo?)* que marcan una diferencia en la libertad real que recibe el usuario.

Entonces es necesario definir ciertas directrices en la forma en que un software está implementado para poder decir que le da libertad al usuario, no en función de su licencia sinó de cómo está implementado.

Las definiciones siguientes **ver: parece ser solo una definición, y varias características** no pueden ser menos que arbitrarias**todas no**, pero vamos a intentar fundamentarlas en ideas anteriores

## El software claro ##

- Su codigo fuente está escrito de forma clara y comprensible.

- La interface no esconde al usuario la funcionalidad tras múltiples capas de
  abstracción. La idea es que una acción del usuario debe tener efectos previsibles
  en el sistema. 

- La mayor parte de la documentación está integrada en el código en la forma de
  comentarios. O sea, la documentación principal no está dada en forma de tutoriales,
  guías de uso o videos que explican como usar el software.

- Permite acceder de forma programática, o sea, provee una API. Esto permite a los
  usuarios realizar tareas complejas mediante la interacción de múltiples herramientas.

De fondo, lo que está en juego es para qué sirve el software, si le sirve al usuario para automatizar tareas repetitivas, o sea, permitir al usuario hacer algo que le tomaría un tiempo, hacerlo de forma más rápida y fácil. La visión del software obscuro, es que el software debe permitir al usuario hacer cosas sin necesidad de que el usuario sepa de que se tratan.

## Combinatoria ##

Otra clasificación más, nos abre nuevas posibilidades, como software cerrado y claro (como basecamp) o libre y obscuro (como libreofice).