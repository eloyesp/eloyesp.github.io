---
---

Hoy estoy otra vez luchando con electrum y en el medio aprendiendo linux, gpg,
python y a manejarme en sociedad.

Entre otras cosas, me tuve que bajar python, porque la versión que viene en el
sistema es demasiado vieja para algunos y se me puso en la cabeza que quería
verificar el archivo que descargué:

    gpg --verify-files Python-3.7.3.tar.xz.asc 

Pero claro, no tengo la llave pública de quien firmó eso, así que bajo las
llaves y actualizo

    gpg --import pubkeys.txt 
    gpg --refresh-keys --keyserver hkp://pool.sks-keyservers.net

Pero claro, eso depende de tener un keyserver configurado, cosa que no tenía,
así que agrego ese para salir del paso. Después si, descomprimo e instalo:

    cd Python-3.7.3/
    ./configure --enable-optimizations
    make

Pero las optimizaciones estables, no funcionan (por algo eran estables y
opcionales no?).

    ./configure
    make clean
    make
    sudo make install

Bueno, terminado eso, y habiendo clonado el repo de electrum y habiendo
agregado el fork de electrum-fair y algún otro para ver me encuentro en
situación de agregar direnv, pero no funciona virtualenv :(, por [este
error con una librería enum][enum-bug], ya que al parecer más allá de que cada
versión de python instala los paquetes por separado python no se da cuenta y
pide todo.

 [enum-bug]: https://stackoverflow.com/questions/43124775/why-python-3-6-1-throws-attributeerror-module-enum-has-no-attribute-intflag#45716067

Así que terminé usando [las instrucciones de direnv para usar
venv][direnv-venv] y salió andando con:

~~~ bash
layout python-venv python3.7
~~~

Ahí me puse a ver las diferencias entre forks, cosa complicada porque segun
github: "Woah, this network is huge! We’re showing only some of this network’s
repositories." O sea, que hay más forks de lo que da...

Así que cuando miro los cambios en un fork para funcionar con otra moneda son
del orden de las 10 mil lineas, cosa que se explica viendo que hay más de 500
menciones de bitcoin en el código. O sea, el código tiene una fuerte
dependencia con la palabra bitcoin.

Busqué un poco sobre el tema en los issues y al parecer hay una gran
resistencia con respecto a la existencia de los forks y a hacercelá facil.

Pregunté si daba para intentar abstraer la idea de bitcoin en el código y la
respuesta que obtuve fue:

> -- does it makes sense to make changes to abstract that out to a separate file,
>    for example electrum/bitcoin.py where everything that is hardcoded for
>    bitcoin belongs?
> 
> -- Maybe, however Electrum does not have any interest in maintaining code for
>    altcoin wallets
> 
> -- I just want to know if it makes sense in a pull-request with something like
>    that or if that is going to be rejected.... :/
> 
> -- I don't think that would be likely to get merged, but please talk to either
>    ghost43 or ThomasV for an official response

Así que al parecer, no es una cuestión fácil, y yo, que no soy un experto en
python no pienso meterme.

 [direnv-venv]: https://github.com/direnv/direnv/wiki/Python#venv-stdlib-module
