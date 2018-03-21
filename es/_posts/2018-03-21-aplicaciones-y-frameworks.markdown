---
---

La semana pasada nos reunimos para comenzar a trabajar en un presupuesto para
una fundación en la cual necesitan un sistema de gestión, por lo que se
proponía utilizar [tryton][1]. Por otro lado, de forma coincidente, mirando el
proyecto [fair-coin][2], me encontré buscando como colaborar con las páginas
web del proyecto y me encontré con que muchas de ellas usan [drupal][3]. Así
que estuve leyendo un poco sobre el tema, probando algunas cosas, y me encontré
con un problema.

## Esto no es un framework

Cuando entré al [repositorio de fair-coin][4], choqué con la primera
barrera, ahí no había código, no tengo forma de colaborar con código, porque,
claro, no hay código, porque drupal no es un framework, es una aplicación.

Entonces, el código que yo estaba buscando, no estaba en archivos, estaba en
una base de datos, la colaboración en el proyecto queda entonces detrás de un
login de usuario, el sistema de colaboración toma las reglas del CMS.

En el caso de Tryton, la situación es similar, la configuración está en la base
de datos, donde se habilitan y deshabilitan módulos (que hacen las veces de
plugins de wordpress) y se configuran los parámetros. Todas esas
configuraciones se guardan en la base de datos y para trabajar en ellas hay que
tener un usuario y hacer los cambios en producción.

Pero entonces, colaborar en esos proyectos, probar ideas y ver cómo queda
implica clonar una base de datos, y reproducir cambios en uno y otro entorno
siguiendo los clicks en una interfase gráfica, probar a mano los cambios.
"Colaborar" cómo se entiende en un proyecto de software libre es imposible.

## Volviendo al código

Pero entonces, podríamos repensar si queremos seguir guardando este tipo de
datos en una base de datos ¿es una configuración un dato a ser guardado y
consultado por ese medio?

Siguiendo este planteo, los frameworks como Ruby on Rails o Django , plantean
la idea de hacer una aplicación que utiliza una serie de herramientas (un
framework). De esta manera, "la configuración es código" y la colaboración se
da como en cualquier proyecto.

Del mismo modo estan ganando popularidad los [generadores de sitios
estáticos][5] e incluso hay algunos [CMSs][6], o sea interfaces para usuarios
de a pie, para editar el contenido.

Esta idea de gestionar la configuración como código, está también presente en
el cambio entre SysAdmin y DevOps, que son básicamente lo mismo, pero mientras
que unos gestionan configuraciones de forma directa, los segundos guardan las
configuraciones en repositorios.

La cuestión, es que el código permite replicar, compartir, probar y comparar de
un modo que una base de datos no permite.

 [1]: https://www.tryton.org/
 [2]: https://fair-coin.org/members
 [3]: https://www.drupal.org/
 [4]: https://git.fairkom.net/drupal/faircoin/
 [5]: https://www.staticgen.com/
 [6]: https://headlesscms.org/
