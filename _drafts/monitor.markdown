---
title: Monitor
layout: post
---

Hay cosas en ruby que no estan buenas, algunos lugares oscuros que dan hasta
miedo.

Hoy estuve mirando la clase [`NET::FTP`][ftp] y tenía un incomprensible llamado
a super en el inicialicer. `FTP` no tiene padres más que `Object`, así que daba
para mirar que pasaba ahí.

Bueno, es predecible, hay un Mixin

 [ftp]: http://ruby-doc.org/stdlib-2.1.2/libdoc/net/ftp/rdoc/Net/FTP.html#method-c-new
 [monitor]: https://github.com/ruby/ruby/blob/trunk/lib/monitor.rb
 [doc]: http://ruby-doc.org/stdlib-2.1.2/libdoc/monitor/rdoc/MonitorMixin.html#method-c-new




