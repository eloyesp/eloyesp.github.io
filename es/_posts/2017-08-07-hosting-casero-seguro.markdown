---
---

Ayer finalmente logré configurar el host en mi casa, pero además logré hacerlo
con https, o sea con una capa de seguridad sobre http. Para hacerlo tuve que
usar _Let's Encrypt_ cosa que no fue tan facil como prometía.

Primero configuré el servidor web, en este caso nginx, agregando en `/etc/sites-available/www`:

```nginx
server {
	server_name poderosa.nsupdate.info;
	root /var/www;
	index index.html;
	location / {
		try_files $uri $uri/ =404;
	}
}
```

Después copié el blog en `/var/www` y el sitio ya estaba disponible en
localhost y desde poderosa.nsupdate.info si ya seguimos las instrucciones del
post anterior. Pero como necesitamos habilitar también el puerto seguro, vamos
a tener que forwardear el puerto 443 (que es el que usa https.

```sh
upnpc -r 443 tcp
```

Después de eso, para gestionar el certificado es necesaria una aplicación
llamada `certbot` que viene a automatizar el proceso. Inicialmente, necesité
agregar un _ppa_ o sea un repositorio de aplicaciones extra. Después de eso, generar el certificado fue bastante sencillo.

```bash
add-apt-repository ppa:certbot/certbot
aptitude update
aptitude install python-certbot-nginx 
certbot --nginx
```

Una vez terminado ese proceso, finalmente el sitio es accesible desde internet
en <a href="https://poderosa.nsupdate.info">https://poderosa.nsupdate.info</a> (pueden entrar).
