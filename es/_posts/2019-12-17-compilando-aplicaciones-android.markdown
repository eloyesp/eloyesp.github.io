---
---

Estuve muchas horas intentando compilar una aplicación Android y al parecer lo
logré, pero quiero documentar que hice, quizás a alguien le evite alguna
frustración.

# Instalar dependencias

Al parecer, las dependencias para compilar aplicaciones en android son un lío,
porque se necesita java, JRE, el Android SDK, gradle y no se cuantas cosas más.

Para hacerlo más complicado, es necesario instalar java en la versión 8, porque
la 11 no funciona, pero es la que se instala, por lo que hay que instalar las
dos:

``` bash
sudo aptitude install gradle android-sdk openjdk-8-jre-headless \
  openjdk-8-jdk-headless fdroidserver

```

Pero con eso no alcanza, porque como vimos, tenemos dos versiones de java y
queremos usar la 8 y gradle no se da cuenta, por lo que necesitamos usar
`update-alternatives`, pero, como java es muy complicado, necesitamos un
comando especial:

``` bash
update-java-alternatives -l
sudo update-java-alternatives -s java-1.8.0-openjdk-amd64
```

Además de eso, por las dudas, yo use algunas variables de entorno:

``` bash
export ANDROID_HOME="/usr/lib/android-sdk"
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
export JRE_HOME="/usr/lib/jvm/java-8-openjdk-amd64/jre"
```

Con todo eso, ahí si, pude ejecutar gradle, pero al parecer, necesita descargar
dependencias, para lo cual tenemos que aceptar los terminos de alguna licencia
y tener permisos de escritura en el directorio dónde está el android-sdk:

``` bash
git clone https://github.com/Shadowstyler/android-sdk-licenses.git /tmp/android-sdk-licenses
sudo cp -a /tmp/android-sdk-licenses/*-license /usr/lib/android-sdk/licenses
sudo chown -R root:sudo /usr/lib/android-sdk
sudo chmod -R g+w /usr/lib/android-sdk
```

Y ahí, aunque nadie pueda creerlo, están todas las dependencias en su lugar...
excepto, que me dió un error al querer hacer algo con unas imágenes, para lo
cual parece que me faltaba un paquete: `libbatik-java` (ni idea que hace, pero
ya tengo sueño para averiguarlo).
