---
---

Un domingo más frente a la compu intentando ver cómo hacer una página web que
reciba pagos online usando criptomonedas, al parecer no era tan fácil.

Yo más o menos sabía lo que estaba buscando, quería algo como
[klukt][] que permite recibir un pago y enterarse cuando el pago
fue recibido. Pero nada es tan fácil como parece, este sistema, por ejemplo
parece que utiliza [una librería][1] que depende a su vez de una serie de
[servidores insight, que están hardcodeados en el código][2], lo cual estaría
queriendo evitar. (unas 20 horas más tarde pienso que quizás me puse muy
exquisito).

Lo cual me dejó pensando que lo que yo quiero es copiar el funcionamiento de
las billetera de android, que se conecta a la red y verifica las transacciones
allí, sin depender de servidores centrales. Pero de alguna manera, no baja toda
la cadena de bloques. La cuestión es que en realidad, no se como funciona, así
que esta vez me puse a leer.

Encontré dos fuentes de información más o menos ordenada que me permitieron
entender (un poco), el panorama, por un lado, la [guía del desarrollador][3] y
por otro la [referencia para desarrollo][4]. Ahí pude entender finalmente que
son los `OP_CODES` y los `P2PKH`, los detalles sobre el consenso y la
confirmación.

Pero, además me permitió ver que el universo de las criptomonedas, tiene mucha
menos variedad de software de la que uno esperaría. Hay un software principal,
que es `bitcoind` que es el nodo completo, implementado en c++, [bitcoinj][] que
es el cliente liviano, implementado en Java (y utilizado por los monederos de
celular) y `electrum`, escrito en python, que es un cliente liviano y rápido que
funciona en una sub-red de nodos electrumx.

Más allá de esas tres billeteras principales, el resto son pequeñas librerías
con fines muy específicos, como [libbtc][] que es una librería para generar y
transformar valores entre distintos formatos (en realidad hace bastante más que
eso, pero no sirve para lo que yo necesito).

En realidad, hay [mucho software][5], pero las librerías para conectarse a la
red principal, o sea al protocolo bitcoin, sólo estan bitcoind y bitcoinj, más
aún conectarse para probar y aprender es muy difícil, ya que el protocolo es
más bien complejo y require interacciones constantes, ya que espera que la
aplicación hable y responda el protocolo para poder probar y aprender (haciendo
casi imposible aprender).

Por lo menos, a esa conclusión llegué al leer la respuesta a [mi pregunta][6]
en el stack-exchange de bitcoin, en el cual me dicen que la idea es que no me
meta en el protocolo y use solamente RPC.

Por lo tanto, al parecer, el camino a seguir es instalar un nodo utilizando
`bitcoind` o `bitcoinj` y utlizarlo para validar las transacciones. Si se
utiliza bitcoind, es necesario utilizar la interfase RPC y en bitcoinj no estoy
seguro.

[1]: https://github.com/guerrerocarlos/bitcoin-receive-payments
[2]: https://github.com/guerrerocarlos/bitcoin-live-transactions/blob/master/index.js#L12
[3]: https://bitcoin.org/en/developer-guide
[4]: https://bitcoin.org/en/developer-reference
[5]: https://en.bitcoin.it/wiki/Software
[6]: https://bitcoin.stackexchange.com/questions/87355/curl-json-rpc-to-the-network-for-learning/87357

[bitcoinj]: https://bitcoinj.github.io/
[libbtc]: https://github.com/libbtc/libbtc
[klukt]: https://www.klukt.com/
