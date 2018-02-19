---
title: PGP en el celular
---

Desde hace ya un tiempo vengo dando vueltas con la cuestión de aprender desde
el lado práctico algo sobre criptografía. Pasa que por alguna causa que me
cuesta explicar el tema me resulta importante y la gente que me rodea pasa de
eso y bueno, la criptografía no es un tema al que se pueda entrar solo.

En los últimos tiempos logré ciertos avances al respecto sobre los cuales
quiero comentar algo. Por un lado generé una clave pgp, la generé en la compu
del trabajo, porque gnupg está más actualizado ahí. El comando para eso es:

     gpg --full-generate-key

Y después de eso dejé todas las opciones por defecto y una _passprase_ mas
bien larga pero muy fácil de recordar.

Este procedimiento y un cliente de correo deberían ser suficiente para ya
muchas cosas. Pero hay muchos conceptos que cuesta entender para poder
entender que está pasando, así que comento algunas cosas que me encontré:

Las claves tienen un id, un long id y un fingerprint, de esos, pero en
realidad son todos el mismo:

    $ gpg --list-secret-keys 
    /home/eloyesp/.gnupg/secring.gpg
    --------------------------------
    sec   3072R/0x4E3A1C30064E6742 2018-02-05 [[caduca: 2020-02-17]]
    Huella de clave = 0FE6 053E A175 B88B 5546  C601 4E3A 1C30 064E 6742
    uid                            Eloy Espinaco <eloyesp@gmail.com>
    ssb   3072R/0x1459C40BEF61947E 2018-02-05
    ssb   3072R/0x9378ABA7E8A3ECAA 2018-02-17

Este comando lista nuestras llaves, el id que aparece en la primera linea:
`0x4E3A1C30064E6742` es el ID largo, el id corto no aparece, pero es:
`0x064E6742`, los últimos 8 numeros de ese. El fingerprint, es simplemente el
número entero (y es el único que hay que usar). Por lo tanto, la clave sería:
`0x0FE6053EA175B88B5546C6014E3A1C30064E6742` (o sea la huella de la clave sin
espacios). ([fuente][1])

 [1]: https://security.stackexchange.com/a/84281

Después de eso, viene la cuestión de las subclaves, la idea es que las
subclaves son claves con uso limitado y uno las puede descuidar un poco más,
arriesgando menos.

## Como usar PGP en el celular

Una de las cuestiones que me invitó a volver a intentar toda esta cuestión de
gpg, fue la de encontarme con que el celular bajaba los emails, por lo tanto,
era candidato a utilizarlo. El problema es que dejar las llaves privadas en el
celular es riesgoso ya que el celular es de google.

Lo que se me ocurrió entonces es utilizar subclaves, la idea es generar
subclaves para encriptación y firma digital y exportar sólo esas al celular,
de forma que si algo parece inseguro creo nuevas subclaves y revoco las
anteriores sin más.

El procedimiento es el siguiente:

1. Generar las subclaves de encriptación y firma, en mi caso, la de
   encriptación ya estaba generada:

       gpg2 --edit-key eloyesp@gmail.com addkey

2. Hacer un backup de las claves:

       cp -a .gnupg/ .gnupg.celular

3. Cambiar la _passphrase_ de la clave que uno va a exportar

       gpg2 --homedir ~/.gnupg.celular --edit-key eloyesp@gmail.com paswd

4. Generar un password para mover la clave de forma segura al celular

       gpg2 --armor --gen-random 1 20 > clave_temporal

5. Exportar la clave

       gpg2 --armor --homedir .gnupg.celular/ --export-secret-subkeys eloyesp |
       gpg2 --armor --symmetric --output clave_celular.sec.asc --passphrase-file clave_temporal --batch

6. Transferir la clave al celular, e importar la clave en [OpenKeyChain][2] y
   desencriptar con la llave simétrica (que está en el archivo clave_temporal)

 [2]: https://www.openkeychain.org/
