---
---

Por alguna causa, siendo que escuché hablar de `logrotate` hace mucho y que
incluso lo configuré alguna vez, nunca se me ocurrió usarlo para los logs de
desarrollo y de pronto, son muchos.

En realidad, era muy fácil, pero lo dejo acá por las dudas:

``` conf
# /etc/logrotate.conf

# al final:
/home/eloyesp/projects/*/log/*.log /home/eloyesp/projects/*/*/log/*.log /home/eloyesp/projects/*/*/*/log/*.log {
  daily
  nocompress
  copytruncate
  minsize 500k
  missingok
}
```

Esto indudablemente va a dejarme logs útiles 4 o más días (si no sobrepasan los
500kb) y con copytruncate no debería molestarme mientras trabajo.
