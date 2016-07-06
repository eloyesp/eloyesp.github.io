---
title: Cambiando las aplicaciones por defecto
---

Estaba trabajando y me encontré con que al abrir de una manera rara un navegador, en vez de "firefox" se abría "chrome", cosa que me pareció un poco molesta.

El problema es que yo tengo ambas aplicaciones instaladas ¿cómo sabe el sistema cual quiero que abra una página web?

Al parecer hay un mambo bastante grande para manejar eso, por un lado, al instalar una aplicación (ej: "iceweasel"), también se instala el archivo `/usr/share/applications/iceweasel.desktop`:

```ini
[Desktop Entry]
Encoding=UTF-8
Name=Iceweasel
Name[es]=Iceweasel
Comment=Browse the World Wide Web
Comment[es]=Navegue por la web
GenericName=Web Browser
GenericName[es]=Navegador web
Exec=iceweasel %u
Terminal=false
X-MultipleArgs=false
Type=Application
Icon=iceweasel
Categories=Network;WebBrowser;
MimeType=text/html;text/xml;application/xhtml+xml;application/xml;application/vnd.mozilla.xul+xml;application/rss+xml;application/rdf+xml;image/gif;image/jpeg;image/png;x-scheme-handler/http;x-scheme-handler/https;
StartupWMClass=Iceweasel
StartupNotify=true
```

En ese archivo hay un montón de datos de la aplicación que se utilizan para generar los menús de aplicaciones y también un campo **`MimeType`** con un listado de tipos de archivo[^1].

Esos datos se *cachean* en el sistema para que al abrir un un archivo o el menú contextual sobre el archivo, nos permita abrir cada uno con la aplicación correspondiente. Las aplicaciones encargadas de generar esa cache y actualizarla son `update-desktop-database` y `update-mime-database`.

El problema es que todavía no entraron en juego las preferencias de uno al respecto, o sea, ¿cómo hago para que mi sistema sepa que yo quiero usar tal aplicación como preferida? Esas preferencias se guardan en el archivo `/etc/xdg/mimeapps.list`:

```ini
[Default Applications]
x-scheme-handler/http=iceweasel.desktop;chromium.desktop;
x-scheme-handler/https=iceweasel.desktop;chromium.desktop;
```

Una vez que cambiamos ese archivo es necesario reconstruir la caché, para lo cual hay que ejecutar:

    $ update-mime-database    /usr/share/mime

Con eso es suficiente para configurar el sistema, pero ¿qué pasa si hay diferentes usuarios con distintas preferencias? En ese caso, todos los archivos que se mencionaron se pueden pisar dentro del home del usuario.

Para cambiar las preferencias, se puede crear el archivo `~/.config/mimeapps.list` y la caché se debe escribir en `~/.local/share/mime` (en ves de `/usr/share/mime`).

 [^1]: Los tipos de archivo mime son una forma de identificar los archivos alternativo a la extensión, que se usa porque la extensión es muy facil de cambiar y confundir a las aplicaciones que intentan abrir un archivo.
