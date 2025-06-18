Bueno, todo inicia con una llamada de una cliente, por un reporte que salió
mal, mientras intento encontrar una causa me choco con otro problema, el
reporte que salió mal tarda mucho tiempo.

Así que miro los logs, en producción, en una maquina vieja digamos, el reporte
demora unos 2 minutos... claramente inaceptable... le comento a la clienta que
me dice que ese era un reporte chiquito, hay otros que tardan más. Es extraño
como los usuarios se acostumbran a sistemas que andan mal.

Así que me puse a mirar, mi primera teoría fue que faltaba un indice en la base
de datos, pero eso no arregló el problema, así que el problema tenía que estar
en otro lado y tirando tiros al azar parece que no le estaba pegando.

Así que instalé (no sin dificultad) [rbspy][], que cuando corrí mi script me
generó este hermoso gráfico:

![flamegraph](/images/rbspy.flamegraph.svg)

Me tomo un rato entender todo ese gráfico, pero marca bastante bien el problema
de performance, que resulta que está en el método
`format_titular_and_pasajeros_count`, y más específicamente en su llamada a
`count`... algo estaba mal ahí. Bueno, el problema era medio tonto, `count`
está llamando a la base de datos, sin importarle que el dato ya está cargado en
memoria, reemplacé ese llamado por `size`, que si usa el _cache_ y la
performance mejoró de forma muy considerable.

Dejo también el [Pull Request][pr] por acá por si alguien quiere chusmear.

[pr]: https://github.com/bluelemons/representaciones/pull/35
