---
title: BTC a BCH usando Thorchain y Electrum
---

Bueno, presento una forma muy simple, pero no muy fácil de cambiar BTC por BCH,
usando Thorchain (que es algo así como un DEX multicadena) pero sin una
interfase específica, usando simplemente [Electrum]. La ventaja de este medio es
que:


- Es (bastante) seguro.

- Es (bastante) económico. (siempre y cuando el monto a cambiar no sea muy bajo)

- No tiene comisiones de la interfase. (porque no estamos usando)


Obvio que también tiene desventajas:


- No tiene privacidad (thorchain es público y es fácil vincular los BTC que
   ingresan con los BCH que salen).

- No sirve para montos bajos.

- Requiere pasos a mano.

- Un error puede costar dinero.

- Utiliza monedas enteras, un solo input por transacción que se usa entero (sin
    cambio).


## Paso a paso

Lo primero es obtener una [cotización] (para ver si nos conviene), por desgracia
la interfase para hacerlo es horrible (pero no es compleja), todo lo que
necesitamos es visitar la URL siguiente, reemplazando el monto que queremos
transformar en satoshis:

    https://thornode.ninerealms.com/thorchain/quote/swap?from_asset=BTC.BTC&to_asset=BCH.BCH&amount=[monto_en_sats]&streaming_interval=5&streaming_quantity=2&tolerance_bps=50

Eso nos va a mostrar un JSON muy mal escrito, que tiene unos datos que nos interesan:


. `inbound_address`: Una dirección a dónde vamos a mandar BTC.

. `expected_amount_out`: El monto aproximado que vamos a recibir en satoshis (de BCH).


Si ese último dato nos gustó, lo que tenemos que hacer es escribir el
["memo"][thorchain-memo] para comunicarnos con thorchain, que debería tener la
forma:

    =:c:[direccion_bch]:0/5/2

Ésto significa, hacer un SWAP (`=`), hacia BCH (`c`), a la dirección indicada.
Luego vienen los parámetros que son muy complicados, pero que yo uso `0/5/2` y
funcionan bien.

El problema es que el memo necesita estar estar escrito en Hexadecimal como un
OP_RETURN, para lo cual podemos usar [rapidtables] o el comando en linux:

    echo -n "memo" | xxd -p

Bueno, con los datos que tenemos ya podemos ir a Electrum a preparar la
transacción.

Lo primero es elegir la moneda que vamos a transformar, para lo cual vamos a ir
al tab de "monedas" y vamos a "agregar a coin control"

Luego vamos a ir a enviar y en el campo "destinatario" vamos a _pegar_ el texto
siguiente:

    [inbound_address],!
    script(OP_RETURN [HEXA_MEMO]),0

Acá estamos usando una [sintaxis especial para escribir memos en
electrum][memo], en la que hacemos una transacción con dos _outputs_: a la
"inbound address" (que obtuvimos en el quote) le vamos a mandar todo (!) y la
otra salida indica que vamos a mandar un memo (que es básicamente un envío a una
dirección OP_RETURN inválida, a dónde mandamos 0 BTC.

## Precauciones **antes de enviar**

Hay que revisar que la dirección "inbound_address" no haya cambiado, para lo
cual hay que refrescar el JSON que obtuvimos al principio, porque si no **los
BTC se podrían perder para siempre**.

Luego hay que enviarlo con un fee "alto", porque si se estanca mucho tiempo
también **puede perderse para siempre**.

## Después de enviar

Bueno, ahí empieza la dulce espera, en general el intercambio va a ser bastante
rápido, o sea que los BCH deberían llegar unos segundos después de confirmarse
la transacción de BTC, pero hay [muchas causas por las que podría tardar
más][delays], para hacer el seguimiento es posible visitar la [página de
seguimiento de thorchain][track].

Si algo salió mal, en general el swap va a "rebotar", o sea que thorchain te va
a mandar la plata (menos los fees).

  [cotización]: https://dev.thorchain.org/swap-guide/quickstart-guide.html#2-query-for-a-swap-quote
  [thorchain-memo]: https://dev.thorchain.org/concepts/memos.html#swap
  [electrum]: https://electrum.org/
  [memo]: https://bitcoin.stackexchange.com/a/122917
  [track]: https://track.ninerealms.com/
  [rapidtables]: https://www.rapidtables.com/convert/number/ascii-to-hex.html
  [delays]: https://thorchain-university.medium.com/under-the-hood-thorchain-transaction-delays-250d00ed57b7
