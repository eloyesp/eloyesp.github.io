---
---

Estuve mucho tiempo buscando hacer andar workrave en la nueva compu, ya que a
alguien se le ocurrió que el paquete de debian debe requerir xfce4-panel, en
vez de recomendarlo o sugerirlo así que tuve que modificarlo un poco.

Como fué esto... al final no fue tan dificil, ya que en realidad no necesitaba
emparchar el workrave, solo cambiar las dependencias, para lo cual use este
script que copié de [este foro][1]:

```bash
dpkg-deb --verbose --extract "$DEBFILE" $TMPDIR
dpkg-deb --control "$DEBFILE" "$TMPDIR"/DEBIAN
vi $TMPDIR/DEBIAN/control
dpkg -b "$TMPDIR" "$OUTPUT"
```

O sea, extrae el deb y de ahí saca el archivo de control, lo modificas, cambias
las dependencias y lo volves a armar. Después de eso, el paquete ya se puede
instalar felizmente con `dpkg -i "$OUTPUT`.

 [1]: https://ubuntuforums.org/showthread.php?t=636724
