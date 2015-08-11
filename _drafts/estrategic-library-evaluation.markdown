---
layout: post
title: Categorías para la evaluación de dependencias.
---

Durante el desarrollo de aplicaciones, la elección de las dependencias es en
general una decisión problemática. De la elección correcta depende en gran
medida el éxito de una aplicación o su fracaso, sin embargo la decisión suele
tomarse en gran medida por intuición, por gustos o por experiencia de los
programadores, no existen parámetros para evaluar las alternativas y predecir
el impacto sobre la aplicación a mediano-largo plazo.

Quizás se deba a la idea del relativismo extremo, según la cual la respuesta es
<q>hay que ver cada caso particular</q> o la visión (para mi simplificada) de
que es <q>una cuestión de gustos</q>. Por el contrario, creo que si es posible
sacar algunas categorías que nos permitan argumentar en un marco más
científico, si una dependencia es más conveniente en un caso particular, que
otra alternativa.

## Funcionalidades

En primer lugar, el uso de una dependencia se basa en un conjunto de
funcionalidades que nos sirve en nuestra aplicación y que por lo tanto nos
saca trabajo de encima, todo aquello que esté resuelto por esta es un problema
menos que solucionar y esta es la características que hace a las librerías tan
atractivas y ubicuas.

Sin embargo, esa funcionalidad tiene una contracara, esa funcionalidad
siempre tiene un costo que deberemos pagar y que debemos tener en cuenta,
vamos a analizar estos puntos de a uno:

Por un lado, acompañando a estas funciones que nos interesan, vienen otras
que no nos interesan, y por más que uno se esfuerce, ese código tiene la
posibilidad de introducir errores, por lo tanto, toda función que no utilizamos
de una librería que utilizamos es un punto en contra de esa librería.

En segundo lugar, el que las funcionalidades ya estén implementadas, no hace
que el código sea simplemente gratuito, el código que importamos en nuestra
aplicación aumenta la complejidad y sus dependencias se vuelven nuestras
dependencias, por lo tanto el costo de agregar complejidad debe ser siempre
tenido en cuenta como punto en contra de la misma.

En resumen, la funcionalidad que nos interesa es un punto a favor, todas las
que no nos interesan son puntos en contra y el código y sus dependencias son
puntos en contra.

---

Pero, al hacer una elección, hay siempre pros y contras,
cuestión que analisis de 
La primera de las categorías de análisis es la de las fu, por la cual me parece importante
comenzar indagando cuales son las cualidades que justifican el uso de
librerías/gemas/módulos en primer lugar, para luego pasar al impacto negativo
de las misma

---

Yo pienso que no, no es solo una cuestión de gustos y se puede encontrar
algunas estrategias para elegir el mejor para cada situación. Es entonces que
pienso que hay que ver cuales son las cosas de cada lado de la balanza y cuales
son las categorías a tener en cuenta.

Vamos a separar este tema en tres entradas intentando analizar en cada entrada
una de estas categorías a tener en cuenta ya que cada una tiene sutilezas que
cobran sentido y valor al ser cruzados con las características de la aplicación
en la que estamos trabajando.

Las categorías son, las funciones, la sinergia y el aislamiento.
