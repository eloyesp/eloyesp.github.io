---
title: Me voy a wordpress!!
layout: post
---

Hoy dejo jekyll y me voy a wordpress, después de [blogspot][1] que dejé porque no
tenía [markdown][2], [ghost][3], abandonado por su precio.

Ahora dejo jekyll por la ausencia de comentarios, porque estaba perdiendo mucho
tiempo viendo que le falta, escribir un post implicaba más que solo escribir un
post, algo esta mal ahí.

Vi que wordpress agregó markdown y que todo el código es GPL, me parece que
estoy hecho.

Lo más complicado es como llevarme lo que vengo haciendo, o sea exportar mis
posts de Jekyll e importarlos en WordPress. No es tan fácil como suena, por lo
que necesité ver como es el [formato de importación de WordPress][4] y se me
ocurrió construirlo usando Jekyll, no parecía tan dificil, pero resulta que
necesitaba tener el markdown de los posts, que [no está disponible de una][5], y
necesité [un patch][6] que [no fué aplicado nunca en Jekyll][7].

Al final funcionó bastante bien, usando [este template][8] y usando un [fork de
jekyll con el patch aplicado][9]

 [1]: http://www.blogger.com/
 [2]: http://daringfireball.net/projects/markdown/
 [3]: http://ghost.org/
 [4]: http://devtidbits.com/2011/03/16/the-wordpress-extended-rss-wxr-exportimport-xml-document-format-decoded-and-explained/
 [5]: https://github.com/jekyll/jekyll/pull/2901
 [6]: https://github.com/steelywing/jekyll/commit/581e78756a6092257f4f740f4b58a82d596b610d.patch
 [7]: https://github.com/jekyll/jekyll/issues/2750
 [8]: https://github.com/eloyesp/eloyesp.github.io/blob/master/wordpress-backup.xml
 [9]: https://github.com/eloyesp/jekyll
