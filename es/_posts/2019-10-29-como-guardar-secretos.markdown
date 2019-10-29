---
---

Advertencia: la verdad esto es un trabajo que todavía no está terminado, pero
quiero poder ir avanzando de a tramos y quiero poder guardar lo que voy
haciendo.

---

La idea, es hacer algo como [`tomb`][tomb] o como [`pass`][pass], que permiten
guardar secretos, pero utilizan para eso, principalmente código que ya existe y
lo combinan para poder hacer herramientas funcionales.

La idea, es poder distribuir de forma segura un secreto en varios lugares,
para lo cual, se puede utilizar [`ssss`][ssss]. Pero, para ser útil, es necesario
tener una forma de entregar con instrucciones las partes de la clave y
utilizarla en conjunto con una llave asimétrica, que permita mantener
actualizados los datos (para evitar la necesidad de redistribuir claves cada
tanto).

Entonces, el proceso sería: utilizar `gpg` para generar un par de llaves
privada y pública, guardar la llave pública para encriptar los datos a
almacenar.

La llave privada, se utiliza como entrada en `ssss`, que genera el secreto a
distribuir, utilizar [`od`][stringtohexa] para convertir esos secretos en
hexadecimal y utilizar [`keyphrase`][keyphrase] y `qrencode -i` para hacer las
versiones fáciles para el usuario final.

Una vez hecho este proceso, enviar archivos encriptados con esa clave hace que
un grupo de estos puedan juntarse a ver los contenidos.

Por último, se puede utilizar un sitio basado en [`ssss-js`][ssssjs] para facilitar
el proceso inverso.

 [tomb]: https://www.dyne.org/software/tomb/
 [pass]: https://www.passwordstore.org/
 [ssss]: https://github.com/MrJoy/ssss
 [stringtohexa]: https://stackoverflow.com/a/6791875/337772
 [keyphrase]: https://github.com/ryepdx/keyphrase
 [ssssjs]: https://github.com/gburca/ssss-js
