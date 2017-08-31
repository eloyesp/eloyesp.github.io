---
---

Ya hace dos semanas que vengo usando [sandstorm][1], en el medio se me
ocurrieron varias ideas para escribir algo sobre esto, pero a la vez me
frenaba, porque algo me hacía ruido.

En este viaje a través de sandstorm, tuve que aprender un montón de cosas,
sobre procesos y comunicación entre procesos, jaulas `chroot` y para qué sirve
el directorio `/proc` en el que al parecer se almacena toda la información
relativa a los procesos que están corriendo.

## De qué va Sandstorm

En escencia sandstorm es una aplicación web, desarrollada principalmente en
javascript, que permite a los usuarios _instalar_ aplicaciones web
(desarrolladas para sandstorm) e _instanciar_ estas aplicaciones muchas veces.
El paso de la instalación, es equivalente a la idea de instalar un plugin en
wordpress, la aplicación descarga un código de internet y lo guarda para usarlo
luego. Por otro lado, estas aplicaciones pueden ser instanciadas muchas veces,
por distintos usuarios, cada instancia puede ser ejecutada de manera
independiente y tiene un conjunto de datos propios.

El ejemplo básico es un editor de texto, la aplicación es una, pero cada
instancia es un texto distinto.

La genialidad de este concepto, esta en que se hace mucho más fácil disponer de
un servidor propio con muchas aplicaciones que uno quiera correr y permite de
hecho la instalación de _software libre_ en Internet. O sea, instalar una
aplicación web es una cuestión engorrosa, pero instalando Sandstorm ese
tormento es una sola vez y permite usar muchas aplicaciones web.

## ¿cómo funciona esto?

Cuando uno crea una instancia, sandstorm crea una carpeta para alojar los
archivos propios de esa instancia y crea una jaula especial para esa instancia,
en la cual solo tiene acceso a los archivos propios de la aplicación (solo
lectura) y de la instancia (lectura y escritura), en esa jaula ejecuta la
aplicación y nos permite interactuar con ella a través de un iframe en la
_shell_ de sandstorm.

Bueno, como se ve, ya no es tan poco lo que hace sandstorm, lo interesante es
¿por qué hace todo eso? por una cuestión de recursos y seguridad, el tema de
los recursos es lograr que las instancias esten encendidas solo el tiempo
necesario y el de la seguridad, tiene que ver con correr muchas aplicaciones e
instancias distintas sin confiar plenamente en ninguna. Como las aplicaciones
están aisladas, no es posible que una corrompa los datos en otra y tenerlas a
todas aisladas permite encender sólo la que está en uso.

Luego queda preguntarse ¿qué es la _shell_ de sandstorm?, es la aplicación web
que el usuario ve, o sea la interface, en la cual el usuario se registra,
inicia aplicaciones, crea instancias, etc. En esencia es una aplicación
[meteor][2] que utiliza [mongoDB][3] como base de datos. Dejo como ejercicio
para el lector, opinar sobre esa elección.

## Cuando todo se empieza a poner oscuro

La cuestión se complejiza aún más en el momento en que nos enteramos, que
sandstorm no interactua directamente con la aplicación web, si no a través de
[cap'n proto][4], que es un sistema para realizar llamados a procedimientos
entre aplicaciones, o sea, una forma en la cual una aplicación ejecuta un
método en otra aplicación. La característica especial de cap'n proto es que se
basa en capacidades, que es un objeto que se pasa (como un token de permisos)
de manera que una aplicación solo puede mandarle mensajes a aplicaciones de las
cuales conoce la dirección.

Utilizando cap'n proto, sandstorm interactúa con la aplicación que está
instalada, pero esta aplicación que está instalada y corriendo no es una
aplicación web... en realidad es una aplicación puente que transforma los
mensajes en requests y envía las respuestas como respuestas, para lo cual
utiliza nuevamente cap'n proto, por lo tanto, la aplicación puente debe correr
la aplicación web como un subproceso, complicando levemente el desarrollo de
las aplicaciones para sandstorm.

Como se ve, este último paso, resulta mucho más complejo y en cierta medida
innecesario en este contexto, entre otras cosas, como las aplicaciones deben
ser compiladas para poder utilizar rpc, esto impide que las aplicaciones se
distribuyan en su código fuente, por lo que es necesario que la distribución se
realice mediante paquetes precompilados (y por lo tanto inseguros).

## Inconsistencias

En realidad, este último punto puede entenderse, no desde el punto de vista
técnico, sino desde el punto de vista del quienes desarrollan sandstorm. Gran
parte del desarrollo de sandstorm se realiza desde las universidades como parte
de proyectos de investigación, o son parte de emprendimientos para escalar
sandstorm en sentido vertical. Esto convierte a sandstorm en un campo de
pruebas, en el cual la prioridad es probar nuevas estrategias.

Me enteré de esto cuando pregunté sobre el tema de las licencia (que siendo un
proyecto que promueve el software libre utiliza una licencia liberal) y me
respondieron:

> I do research at a university, code I write for a grant is copyright the
> university they will let me release under some free software licenses, but GPL
> is forbidden.

O sea, que desde la universidad pueden escribir cosas públicas, siempre y
cuando esté permitido utilizarlo para esclavizar a otros. Al parecer esto
termino en una [discusión muy interesante en el chat][5].

De pronto, entender una contracción dejó más clara la otra, sandstorm no es un
proyecto real, es una prueba de concepto.

 [1]: https://sandstorm.io
 [2]: https://www.meteor.com/
 [3]: https://www.mongodb.com/
 [4]: https://capnproto.org/
 [5]: https://botbot.me/freenode/sandstorm/2017-08-24/?msg=90243355&page=1
