---
layout: post
title: "Make specifications more specific"
date: 2012-12-11 15:26
comments: true
categories: 
---

Ahora estoy haciendo el [curso de codeschool sobre rspec] [1].

El primer capítulo da un poco de terminología básica nada nuevo ni interesante (excepto que no tengas ni idea sobre de que se trata).

Pero lo primero que veo es la cuestión de que testear y la respuesta tiene que ver con el código. Lo que se están escribiendo son las especificaciones, o sea las salidas esperadas, lo que se testea son las interfaces. Entonces... como se hace una clase facilmente testeable? se hace una interface muy pequeña, se define con claridad que es lo que la clase tiene que hacer y cuales son las entradas y las salidas.

El objetivo del BDD (y no en general del testing) es conseguir una buena ingeniería de software.

Otra cuestión que es interesante, es que el primer capítulo presenta una syntaxis de rspec que [se está dando de baja] [2].

Acá pego un ejemplo por si no tienen ganas de buscar mucho...

``` ruby
# old syntax
foo.should eq(bar)
foo.should_not eq(bar)
# can be changed for
expect(foo).to eq(bar)
expect(foo).not_to eq(bar)
```

  [1]: http://rspec.codeschool.com
  [2]: http://myronmars.to/n/dev-blog/2012/06/rspecs-new-expectation-syntax "Blog sobre la nueva syntaxis de RSpec"
