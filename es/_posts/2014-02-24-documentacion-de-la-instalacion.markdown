---
title: Documentación de la instalación.
date: '2014-02-24 19:30:00'
---

Bueno, el tema ahora es escribir la documentación, pero bajo el concepto de que la documentación debe estar en el código.

Pero obviamente no es tan fácil. ¿Dónde va la configuración de los deployments? Y hay un inconveniente, porque a pesar de que la aplicación es una aplicación web, el único lugar en el que se hace referencia a los servidores, es en la configuración, entonces, algo no está tan bien.

Una opción que se me ocurrió, es utilizar un modulo, para todo lo que tenga que ver con Deployment (por lo tanto en `lib/deployment.rb`), luego, en `config/deploy.rb` (¿alguien dijo [capistrano][cap]?).

La opción que probé, es escribir las instrucciones básicas en un archivo `DEPLOYMENT.md` en el root de la aplicación, explicando los requisitos y los archivos que había que leer y modificar.

Luego, hay que agregar una última linea, en el archivo `.yardopts`.

	--files DEPLOYMENT.md
   


  [cap]: http://capistranorb.com/
