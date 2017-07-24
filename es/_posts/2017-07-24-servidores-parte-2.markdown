---
title: Servidores (parte 2)
---

Esta es la continuación de [mi intento de configurar un servidor rails][1], hoy
logré pedir ayuda y no me resolvió todo, pero al menos me parece que avancé.
Creo que lo que más me ayudó fue que de pronto me pusieron en cuestión si en
serio quería usar capistrano, al final lo estoy usando, pero sólo entender que
no es necesario me permitió ver las cosas desde otro lado.

Usé una solución un poco más simple, evitando el tema de tener un servidor web
y uno de aplicación usando passenger standalone (que usa su propio nginx por lo
bajo), para lo cual sólo fue necesario cambiar en el Gemfile `require 'thin'`
por `require 'passenger'`.

Después de eso la cuestión de reiniciar el servidor se hace mucho más simple:

```ruby
# config/deploy.rb

namespace :deploy do
  desc "Restart server"
  task :restart do
    on roles(:app) do |host|
      execute :touch, release_path.join('tmp/restart.txt')
    end
  end

  after :publishing, :'deploy:restart'
end
```

Por último, queda la cuestión de la configuración de passenger, que se resuelve
con un solo archivo, que se guarda en shared: `append :linked_files, 'Passengerfile.json'`

```json
{
  "environment": "production",
  "port": 80,
  "daemonize": true,
  "user": "representaciones"
}
```

Por último, está el problema, que todavía no resolví, que es iniciar el
servidor al encender la máquina (que no debería detenerse nunca). Pero el
script es muy simple (no logro que corra al inicio).

```sh
cd /var/www/representaciones/current
bundle exec passenger start
```

Una vez que logre eso, el servidor debería estar andando.

 [1]: {% link es/_posts/2017-07-03-servidores.md %} "Servidores"


