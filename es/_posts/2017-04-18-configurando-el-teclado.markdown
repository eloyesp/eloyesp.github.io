---
title: configurando el teclado
layout: post
---

Estuve configurando el teclado, hace ya unos meses para poder empezar a
practicar dvorak, pasaron un par de meses y ya ni sabía dónde había puesto esa
configuración. Bueno, al parecer el archivo en cuestión era
`/etc/default/keyboard` que setea algunas variables que usa Debian para
configurar el servidor X.

E aquí mi configuración actual

``` bash
# KEYBOARD CONFIGURATION FILE

# Consult the keyboard(5) manual page.

XKBMODEL="pc105"
XKBLAYOUT="es,us"
XKBVARIANT=",dvp"
XKBOPTIONS="grp:ctrls_toggle,grp_led:scroll,caps:swapescape"

BACKSPACE="guess"

# quizas viene bien: KMAP=es
```

Lo que hace de interesante es, tener dos layouts: 'es' y 'en', cada uno tiene
asociada una variante, el español tiene la variante por defecto, pero inglés
tiene "dvorak-programmer".

Además de eso, están las opciones, que son de lo más crípticas,
grp:ctrls_toggle, permite cambiar el layout del teclado apretando los dos
control a la vez, grp_led:scroll indica cuando está en dvorak con la luz del
scroll lock y caps:swapescape me permite usar vim con felicidad, usando el
capslock como si fuera escape.
