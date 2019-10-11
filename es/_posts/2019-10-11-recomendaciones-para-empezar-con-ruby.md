---
title: Recomendaciones para empezar con ruby
---

Un par de recomendaciones generales a quien está empezando a programar con
ruby, quizás muchas se apliquen a otros lenguajes, pero en este caso, es ruby.

## Usa GNU/Linux

Ruby en windows es un dolor de cabeza, si podés, usa GNU/Linux, instalalo en
serio, no en una máquina virtual porque es muy lento, si venís de windows y no
sabes con que empezar, te puedo recomendar [Mint][].

Esto va a incluir necesariamente aprender a manejar un poco la consola y un
poco de inglés. Algunas cosas que está bueno saber son las siguientes:

 - `man`: un comando que permite leer la ayuda (las páginas del manual), hay
   ayuda sobre los comandos y sobre algunos archivos de configuración.

 - `apropos`: comando que permite buscar páginas del manual por el contenido y
   es una buena forma de buscar otros comandos.

 - `/usr/share/doc`: Es un directo lleno de documentación sobre los programas
   que tenés instalados en el sistema.

 - `history` es un comando de bash que permite leer todos los comandos que
 escribiste antes (y es muy bueno para tomar notas)

 [Mint]: https://www.linuxmint.com

## Aprendé git

Aprendé los comandos básicos de git en la consola (no importa si lo vas a usar
desde ahí, la cuestión es entender de que va). Mi recomendación para todo el
que empieza con git, es hacer, como mínimo un commit cada hora y escribir un
mensaje claro de qué cambio hiciste.

## Usa un editor de texto

Hacete amigo de un editor de texto plano, en Ruby un IDE es un dolor de cabeza
más. Es tan difícil hacerlo andar, que vas a perder más tiempo haciendo andar
el IDE que aprendiendo Ruby. Usa un editor de texto plano, no necesita ser vim,
pero evitá los monstruos que tardan demasiado o agregan muchas funciones.

## Escribí un blog

Intentá escribir, como mínimo, una entrada por semana, en la cual comentás algo
particularmente interesante que aprendiste. Eso te va a ayudar a fijar los
conocimientos.

## Aislar los problemas

Lo más importante al encontrar un problema es aislarlo, identificar con
claridad, qué es lo que se está intentando, que archivos, comandos y
configuraciones tienen que ver, cuales son las lineas de código relevantes, que
intentaste, el resultado que esperabas y el que obtuviste.

Para ayudar a identificar eso, lo primero es intentar sacar todo lo que no
tiene que ver directamente con el problema, ir limpiando el código o
directamente, construir otro proyecto dónde sólo esté el código necesario para
reproducir ese problema.

## Pedir ayuda

En general, en este proceso, el problema se hace visible y se puede resolver, si
no, llegó el momento de pedir ayuda, para lo cual se puede usar una lista de
correo cómo [rubysur][] o un sitio como [StackOverflow][]. Enh

La prioridad es que estén los tres elementos: qué es lo que querés logar, cómo
lo intentaste y qué obtuviste. La segunda prioridad es que sea corto. Más allá
de eso, es importante que esté bien escrito.

También es de buen gusto, leer con atención las respuestas y dar feedback.

 [rubysur]: https://groups.google.com/forum/#!forum/rubysur
 [StackOverflow]: https://es.stackoverflow.com/
