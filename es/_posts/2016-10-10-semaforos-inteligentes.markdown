---
title: Semaforos inteligentes
---

Esta es una idea de IoT o de electrónica o algo así... la idea es hacer
semáforos con sensores, que midan la cantidad de personas que circulan en cada
sentido y cambien para optimizar el transito.

Inicialmente no creo que sea necesario ningún tipo de controlador central, ya
que la lógica es muy simple y se puede realizar en el mismo semáforo, sólo se
necesita comunicar el estado actual para coordinar.

La idea es que en una esquina haya cuatro postes de semáforo con un semáforo en
cada sentido (como vi que es en Montevideo). Cada uno de los módulos debe disponer
de un sensor.

El sensor no estoy seguro cual podría ser, la idea es medir la cantidad de
gente que quiere cruzar en cada sentido, ya sea en auto o peatón. Basado en ese
dato se haría un calculo de puntos, tomando como puntos negativos el tiempo que
está en verde y como positivos la gente cruzando, cuando el otro sentido de
circulación gana en la pulseada el sentido de circulación cambia.

Por otro lado, cuando hay pocos datos, o sea no hay gente cruzando en ningún
sentido, cambiaría a las típicas señales intermitentes amarillas que hay en la
noche.

La única cuestión que es complicada es la del sensor, yo pensé en sensores
infrarrojos, pero no se si el costo es aceptable.
