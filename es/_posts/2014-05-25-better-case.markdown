---
layout: post
title: Better case.
date: '2014-05-25 02:11:09'
---

Se me ocurrió una idea para una gema, como pasa cada tanto, el problema es que hacer una gema muy pequeña toma más tiempo del que uno quisiera.

Algo me hace pensar que uno debería tener un script para hacer gemas si quiere hacer gemas bien pequeñas.

Esta es bien pequeña, la idea es hacer que el operador `===` sea un poco más interesante en ruby. Ruby tiene algunas funcionalidades interasantes en ese operador, pero algunos casos son particularmente aburridos.

Por ejemplo, en Array `===` es lo mismo que `===` y me da la impresión de que sería mucho más útil un `includes?`. Los nombres de clases se pueden usar para testear clases (usando `kind_of?`) pero no hay un mecanismo fácil para hacer duck type, mientras que `symbol` está casi al dope. Para mí es bastante obvio que hay que hacer que hacer:

~~~ruby
class Symbol
  def === obj
    obj.respond_to? self
  end
end
~~~

No opinan igual?

Bueno, todavía no hice la gema, pero ya la voy a hacer, a alguien se le ocurre algún buen caso más?
