---
title: ¿cuándo usar bash?
---

Es sabido que los lenguajes tienen una tarea en la que brillan, en el caso de bash, esa tarea es la automatización de tareas en la cual se integran diversas herramientas.

En el día a día como programador uno pasa bastante tiempo en la consola y es como un centro de operaciones en el que cualquier comando está a un comando de distancia, bash tiene a su disposición el mismo centro de operaciones.

## Herramientas

Cuando pienso en las herramientas que usamos a diario me olvido de varias, así que acá hago una lista:

- git

- gzip

- tar

- make/rake/grunt/gulp

- apt/aptitude

- vim/emacs/\$EDITOR

- mail 

Lo copado de bash es que todas esas cosas son muy fáciles de acceder (aunque algunas requieren preparativos como el mail) y muchas otras se pueden integrar con relativa facilidad (gist, pastebin, twitter, irc, duckduckgo, youtube) simplemente instalando alguna aplicación que nos ayude o simplemente usando curl.

## Ventajas

Las ventajas frente a otros lenguajes se hace evidente cuando miramos como leer un archivo:

~~~ruby
## leyendo_un_archivo.rb
puts File.read 'leyendo_un_archivo.rb'
~~~

~~~javascript
// leyendo_un_archivo.js
fs = require('fs');

fs.createReadStream('leyendo_un_archivo.js')
  .pipe(process.stdout)
~~~

`cat file.txt`

## Streams

Bash es una gran herramienta pero igual hay cosas que cuesta procesarlas ahí dentro, hay cosas que es más cómodo hacer en ruby o en javascript, pero esas herramientas se vuelven más complejas si no podemos integrarlas a nuestros scripts de bash. Así que lo que nos falta es aprender a jugar en nuestras mini aplicaciones a jugar con las reglas de la terminal.

~~~ruby
## filter.rb

while gets do
  puts $_.gsub(/a/,'b')
end
~~~

~~~javascript
// filter.js
var split = require('split');
var line_number = 0;

process.stdin
  .pipe(split())
  .on('data', function (line) {
    process.stdout.write( ++line_number + ': ' + line + "\n");
  })
~~~

Las reglas de la terminal es que toda aplicación es un filtro, recibe un input (`$stdin` o `process.stdin`) y escribe un output (`stdout`), de esta manera nuestras aplicaciones pueden ser reutilizadas en nuestros scripts de bash.

## Dos scripts en vez de uno

La ventaja que tiene hacer la mitad del script en bash y la mitad en ruby/javascript/loquesea es grande y vale la pena, la complejidad del script de bash que solo hace de pegamento entre herramientas es muy baja, la complejidad del otro script cuando le sacamos de encima toda la complejidad de abrir archivos, bajar cosas, etc. también disminuye y además, el segundo script de naturaleza más general es más fácil de reutilizar.
