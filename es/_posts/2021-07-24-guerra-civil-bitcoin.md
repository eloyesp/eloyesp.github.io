---
tags:
  - bitcoin
  - criptomonedas
  - censura
  - conspiranoia
---

La historia empieza con unos muy esperados videos sobre Bitcoin, que comenzaron
para mi sorpresa con una [serie de videos que desbancan _mitos_ sobre el
mismo][1]: muy estudiadas, muy bien explicadas. Hay un problema solamente,
algunos de los argumentos que presentan: son falsos.

En esta entrada, voy a intentar comentarlas un poco y voy a intentar aprovechar
para contar algunas partes de la historia que no son tan populares en español.
Ellos (ya veremos quienes), están intentando reescribir la historia. Al fin y
al cabo, el vencedor siempre aprovecha ese lugar para escribir su historia.
La reescritura, en este caso comienza negando la guerra, así que vamos a
empezar a contar un poco de que se trató todo esto.

En particular, la sección más conflictiva, es la tercera, sobre la la
escalabilidad en Bitcoin, un tema que cobró relevancia entre 2015 y 2017,
en lo que se conoció como la guerra civil de bitcoin.

La sección que trata el tema simplifica mucho la cuestión, hace una breve
explicación de que bitcoin no puede escalar simplemente haciendo los bloques
más grandes y ridiculizando la postura con el meme de turno. Todo en solo tres
minutos. Luego concluye (con una falta inigualable de lógica), que bitcoin
escala sin problemas utilizando soluciones de segunda capa, como la lighting
network, liquid o los bancos (no es joda, lo dicen en serio). (Estas falsas
soluciones son analizadas más adelante).

En esos tres minutos en que se ridiculiza la postura de [extender el tamaño][3]
de los bloques, dejando en claro que es una opinión absurda y minoritaria, se
plantea que omiten un punto elemental, que es el costo para los mineros de los
discos rígidos, que al ser muy elevados terminarían centralizando mucho la red.
Desmentir ese argumento, requiere solamente una búsqueda en internet, en la
cual se compara el costo del [hardware de minería][2], que cuestan de 1500 USD
para arriba cada uno, con el de los discos rígidos, que están menos de 200 USD.

Pero en esos tres minutos no están ahí para blandir argumentos (quién necesita
argumentos cuando hay memes), están ahí para silenciar a todos los que fueron
derrotados en los sucesivos forks de las cadenas, como Bitcoin Cash, Bitcoin SV
y un gran etc. Dejando en claro, que Bitcoin es el único bitcoin.

## Embrace, extend and extinguish

[Adoptar, extender y extinguir][4], es el nombre dado a la estrategia utilizada
por mícrosoft y _otra buena gente_ para destruir los proyectos libres (tanto de
software como de protocolos), es la forma que utilizó para adueñarse de
Internet. Esta estrategia, fue bien aprendida por sus seguidores de [GAFAM][5],
la idea es adoptar su uso, colaborar y coaptar su desarrollo, luego incorporar
cambios que eliminan la cuestión central pero que parecen opcionales y luego
volverlas necesarias para así generar dependencia.

Para que esta estrategia fuencione, en el caso de bitcoin, eran necesarias dos
cuestiones, por un lado, que bitcoin fuera uno solo (en este momento la
dominancia de bitcoin es del 46%), por otro, asegurar que la primera capa de
bitcoin no fuera escalable, porque de serlo, al ser un sistema descentralizado
no es posible ejercer control.

Entonces comenzaron la censura y los ataques, el reddit de bitcoin fue
censurado a niveles ridículos, bitcoin.org también y así también los
principales foros y finalmente los nodos que mostraron voluntad de cambiar
fueron atacados.

Por lo que sabemos, uno de los desarrolladores, que irónicamente, era el que no
creía que bitcoin pudiera ser un medio de pago, obtuvo la plata para fundar su
empresa Blockstream, que desarrollaría las soluciones de segunda capa. Impuso
el límite artificial en bitcoin para que su segunda capa fuera necesaria.
Seguramente hay algo más en esta historia, pero como no sé qué es, prefiero
dejarlo acá.

Lo unico que se, es que tanto la persona a la que Satoshi delegó el proyecto:
Gavin Andresen, que fue también uno de los principales impulsores del aumento
del tamaño del bloque, como [Mike Hearn][6], otro de los primeros, abandonaron el
proyecto después de incansables luchas con respecto a este asunto junto con
muchos otros. Por otro lado, muchos de los otros desarrolladores fueron
contratados por blockstream en el proceso.

## Extender

Ahora voy a intentar explicar como funcionan estas soluciones de segunda capa
propuestas por blockstream, ya que incorporan muchos términos complejos que
esconden lo que realmente está pasando.

La primera idea, es utilizar una side chain llamada liquid. Esa side chain
funciona liderada por un _consorcio de entidades_ que tienen control de esta
cadena, lateral. Los bitcoins se pasan desde la cadena principal a la lateral
depositándose en las cuentas de estas _entidades confiables_ en la cadena
principal y como la cadena lateral es centralizada, no tiene límites como la de
bitcoin.

La segunda es más complicada y requiere términos más complejos, se llama
lighting network y permite hacer pagos muuuy económicos y rápidos, pero
requiere que los usuarios tengan una conexión constante y segura a internet,
canales establecidos con otros nodos, bitcoins suficientes depositados en esos
canales. Como todo eso es realmente muy complicado de hacer además de poco
económico, en general, es necesario _un servicio de terceros_ para acceder a esta
nueva red y software más complejo y difícil de auditar para poder acceder. Por
no hablar de lo difícil que se vuelve guardar los bitcoins con seguridad.

Por último, plantean que utilizar los exchanges de bancos no es tan malo,
cuestión que nos termina de asegurar que los blockstream bitcoins no tienen
mucho que ver con lo que en su momento nos imaginábamos.

Volviendo a los videos, las quinta seccion trata la cuestión de la centralidad
del bitcoin con respecto a otras criptomonedas, en la cual se plantea que a
esta altura del partido, no importa que las otras criptomonedas sean mejores
tecnológicamente, porque el [efecto de red][7], (que es nombrado en EEE) impide
que una ventaja tecnológica pueda afectar al bitcoin como inversión. Dicho de
otro modo, el control efectivo sobre los elementos centralizados del bitcoin
(sitio web, código, dominios), asegura el control efectivo de la red.

## Extinguish

El último paso, entonces, es extinguir toda posibilidad de que bitcoin exista,
como un sistema de pagos p2p, anónimo y descentralizado, para lo cual, el
último paso, parece ser la implementación de taproot en el protocolo, dando
lugar a una brecha de privacidad enorme, ya que si bien se presenta como una
solución de privacidad, al parecer [podría ser completamente lo contrario][8].

## Crítica

Me parece que está claro que el tren de bitcoin perdió sus vías. Quizás sea
buena inversión, pero ya no es un gran proyecto. En realidad, el proyecto de
bitcoin estaba condenado a tener este tipo de altibajos, no hay forma de
imponer la libertad, las personas deben buscar, mediante la crítica constante y
sistemática, liberarse a si mismas. Bitcoin fue un gran paso en esa dirección,
ahora es el momento, de dar el siguiente paso. En este momento, lo mejor que
encontré es [dash][9], que si bien tiene sus defectos, me parece que está en la
vía que blockstream bitcoin abandonó.

[1]: https://www.thebword.org/c/track-1-demystifying-bitcoin
[2]: https://www.buybitcoinworldwide.com/mining/hardware/
[3]: https://github.com/bitcoin/bips/blob/master/bip-0101.mediawiki
[4]: https://es.wikipedia.org/wiki/Adoptar, extender_y_extinguir
[5]: https://es.wikipedia.org/wiki/Gigantes_tecnológicos
[6]: https://blog.plan99.net/the-resolution-of-the-bitcoin-experiment-dabb30201f7
[7]: https://es.wikipedia.org/wiki/Efecto_de_red
[8]: https://github.com/Har01d/talks/raw/master/20201127-Taproot.pdf
[9]: https://www.dash.org/es/
