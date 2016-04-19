---
title: Grandes cambios
---

Trabajar en una aplicación mediana/grande tiene el problema de que más
tarde o más temprano, llega el momento en que uno descubre que algo empezó
con el pie izquierdo, en algún momento a alguien se le ocurrió una genial
idea (en muchos casos ese alguien fue uno) y esa idea resultó no tan genial.

A alguien se le ocurrió usar herencia para hacer que todas las vistas sean una,
porque son todas muy parecidas y con el tiempo esa vista es una nube de ifs
anidados. Ahí empieza el baile, porque cuando tenemos una condición, la única
forma de seguir es agregar más condiciones haciendo nuestra tumba un poco más
profundo cada día.

Esto puede pasar en varios lugares, en una vista que hace muchas cosas, en un
modelo comodín, en el servicio ese que hace de todo, en el modelo de usuario o
en mock que usamos en los tests. Resulta que en algún momento nos damos cuenta
que las cosas se descarriaron y volver a la ruta se hace cuesta arriba.

### Iteraciones

Para solucionar este tipo de problemas, nuestro primer instinto es solucionar
el problema de una, rehacer ese método, repensarlo y arreglar todo, el problema
es que, aunque eso funciona en la teoría y a todo el mundo le parezca genial,
en mi experiencia, nunca es posible.

En vez de eso mi estrategia es tratar al código como a un virus y ponerlo en
cuarentena, de forma que el mismo queda en uso, pero se detiene su avance.
Básicamente se aplican dos reglas a ese código, por un lado ningún código nuevo
puede llamarlo y por otro ningún cambio puede realizarse para agregar
funcionalidad. Sólo se permite modificarlo en caso de un bug absolutamente
urgente.

De esta forma, con esas reglas en su lugar, para continuar el desarrollo se
hace necesario circunvalar el código infectado, crear nuevas interfaces,
vistas, métodos para poder implementar las nuevas funcionalidades. A veces
queda medio raro, ya que hay funcionalidad duplicada, pero al tiempo, una vez
que el código ya no está casi en uso, se hace fácil, dar la estocada final y
borrar ese código que molesta.

Puede ser difícil a veces explicárselo al PM, porque de pronto todas las tareas
que se podían estimar en dos horas, de pronto toman un día, porque hay que
crear todo eso nuevo, pero a la larga, el resultado vale la pena.
