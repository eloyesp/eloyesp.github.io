---
tags: cryptocurrencies
---

There is a known issue on bitcoin, that is that every transaction requires a
new address to prevent [address reuse][], making it uncomfortable to make
recurring payments to known people.

I propose here a solution for that issue that use [BIP32][] derivation to use a
single address exchange to make multiple payments without reusing addresses and
without privacy lost requiring no change on the protocol level, but software
support from both sending and receiving sides.

There is already some mention of handling recurring payments [on the BIP32
itself][1], but it does not propose any specific derivation path standardization
making it hardly usable.

So the idea is to propose an standard, but as it requires a magic number, that
would be the "Purpose" (as proposed by [BIP43][]), lets make an example, using
150 as the supposed BIP number.

Bob want to make recurring payments to Carol, so he ask her for a _contact
address_, that is a public key derivated using the path: `m/150'/i`, where
`150` is the supposed BIP number. She needs to send him the actual xpub key on
that derivation address.

She can record in her wallet that this derivation path is linked to Bob
payments (stored offchain), optionally she could send herself a payment to the
address on `m/450'/i` to mark the contact address as spent (onchain), that will
prevent giving the same public key to multiple people (losing privacy).

Bob can use that public key to generate multiple derived addresses to make
multiple recurring payments to Carol, the contact address is stored offchain,
anyone inspecting the chain will see normal transactions on chain. To make it
possible to send multiple coins the derivation path used by Bob should be:
`c/t/j`, where `c` is the "contact key" given by Carol, and `t` is the "coin
type" as specified on the [slip-0044][].

Notice that this only work on a singe direction and do not make it possible for
Carol to send anything to Bob, as it would require Bob sending her a contact
address.

## Example

Let's suppose that carols have the following master private key:
    xprv9s21ZrQH143K2JF8RafpqtKiTbsbaxEeUaMnNHsm5o6wCW3z8ySyH4UxFVSfZ8n7ESu7fgir8imbZKLYVBxFPND1pniTZ81vKfd45EHKX73

With that, she send the contact key:

    m/150'/1: xpub6A6DyH5QFt4AYhfqGdG5FGVePWTpWr85GjUqb88rsvJ2TPJh9hxxTZ65yFxCLtyTFL3KnkMCVEKNgReohmS24SfEn57mp1nv1X7cHKDPjir

And wait for payments on the addresses:

    m/150'/1/1/1: 1BrGepBjRudRG2iGZVWrzPv1zFbgzbnmes
    m/150'/1/1/2: 18spmXmtqGcvao58dnCZPLMLkurDmfxNMH

Bob, on his side can generate those same addresses using the given contact key
and the derivation paths: `c/1/1` and `c/1/2`.

This can be tested in http://bip32.org/

This can also be used for other coins, so Bob can send Carol Dash without
requesting her a new contact address, using the derivation path `m/5/i` that
Carol can check using `m/150'/1/5/i`, the only issue being that it is not using
hardened derivation for those.

A positive side effect of using this, is that Bob can chose to send payments to
Carol using multiple outputs, that could allow him to have more privacy, at the
expense of Carol having more UTXOs that could make her pay more fees to spend
those coins.

[address reuse]: https://en.bitcoin.it/wiki/Address_reuse
[1]: https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#recurrent-business-to-business-transactions-nmih0
[BIP32]: https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki
[BIP43]: https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki#Purpose
[slip-0044]: https://github.com/satoshilabs/slips/blob/master/slip-0044.md
