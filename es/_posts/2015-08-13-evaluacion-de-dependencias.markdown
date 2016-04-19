---
title: Evaluación de dependencias.
---

La elección de las dependencias es una decisión importante, de la cual depende
en gran medida, el éxito de una aplicación o su fracaso. Sin embargo, la
decisión suele tomarse a la ligera, sin revisión ni discusión. La decisión se
fundamenta en la intuición, en la popularidad y en el marketing. No hay
parámetros claros para evaluar, ni se suelen discutir las alternativas.

Quizás se deba a la idea del relativismo extremo, según la cual la respuesta es
<q>hay que ver cada caso particular</q> o la visión (para mi simplificada) de
que es <q>una cuestión de gustos</q>. Por el contrario, creo que sí es posible
identificar algunas categorías que nos permitan argumentar en un marco más
científico, si una dependencia es más conveniente en un caso particular, que
otra alternativa.

## Funcionalidades

En primer lugar, agregamos una dependencia porque implementa un conjunto de
funcionalidades que necesitamos en nuestra aplicación y por lo tanto nos saca
trabajo de encima, ya que entonces no tenemos que implementarlas. Ésta
es la característica que hace a las librerías tan atractivas y ubicuas.

Sin embargo, esa es solo la mitad de la historia, ya que escribir el código es
sólo parte del trabajo, luego hay que mantenerlo, integrarlo, configurarlo y
adaptarlo a los nuevos requerimientos que vayan surgiendo. Que tan fácil
resulten esas tareas depende, en gran medida, de la *calidad del código* y los
fallos se acumulan como *deuda técnica* y dificultan su uso. Al agregar una
dependencia, su código se vuelve parte de la aplicación y de la misma manera su
deuda técnica se vuelve nuestra.

En el caso de las dependencias, esta cuestión tiene dos problemas agregados
más: por un lado que acompañando a las funciones que nos interesan, vienen
otras que no nos interesan, esas funciones no agregan nada positivo a la
aplicación, pero la deuda técnica sí se hereda.

Por otro lado, las dependencias son transitivas, por lo que la deuda técnica
agregada por una dependencia incluye, a su vez, la deuda de todas sus
dependencias.

## Modularización

En segundo lugar, las dependencias pueden ser una forma de disciplinar el
código, ya que el código de las dependencias está naturalmente aislado del
código principal de la aplicación, de forma que la arquitectura general del
código tiende a ser más modular en cuanto más funciones se derivan a librerías
externas.

El inconveniente es cuando esa separación tiene fallas ya sea porque la
librería invade fuertemente el espacio global, alterando variables globales y
de entorno, haciendo *monkey patching* o alterando el estado del sistema de
alguna forma inesperada. En esas circunstancias, la arquitectura de la
aplicación se resiente y los conflictos se hacen más frecuentes.

Otra posible brecha, es que el uso de la librería quede desperdigado por toda
la aplicación ya que de esa manera, un cambio en la interfase de la librería
provocaría un cambio muy extenso en la aplicación.

En resumen, el uso de librerías externas *puede* ayudar a mejorar la
arquitectura de la aplicación, siempre y cuando, la librería no sea
**contaminante** del espacio global.

## Sinergia

Es importante tener en cuenta que hay librerías que están más orientadas a
evolucionar de forma más o menos constante, mientras otras tienen un fin
preciso y una vez que funcionan suelen mantenerse "estables" por años.

En el caso de las librerías estables no hay una evolución de la librería a
tener en cuenta, pero en el caso de las cambiantes es de suma importancia la
dirección o el *roadmap* de la dependencia que se está integrando.

Si la librería que se integra va en la misma linea que la aplicación, la
sinergia entre ellas puede facilitar futuros desarrollos y la implementación de
nuevas funcionalidades colaborando con otros desarrolladores y facilitando así
la tarea, por el contrario, si los caminos divergen la misma se vuelve cada vez
más difícil de integrar y aumenta el número de funciones inútiles.

# Conclusión

Me parece importante que se discuta un poco más al agregar dependencias y
categorías como "deuda técnica", "contaminación", "estabilidad" y "sinergia"
pueden ayudar en estos debates.

# Links de interés

[Finding modules de Substack][1] y [Popularity de Soberan][2] pueden ser
interesantes para ver los criterios para elegir librerías minimalistas.

 [1]: http://substack.net/finding_modules
 [2]: http://soveran.com/popularity.html
