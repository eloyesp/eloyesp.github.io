---
title: Distributed "Bureau de Change"
---

I'll try to write this idea as a [BIP][] but with FairCoin on [FreeVision][] in
mind. So let's use FVP as the name.

    FVP: ?
    Title: Distributed Bureau de Change
    Author: Eloy Espinaco <eloyesp@gmail.com>
    Comments-URI: https://github.com/eloyesp/eloyesp.github.io/wiki/Comments:FVP-distributed-bureau-de-change
    Status: Draft
    Type: Process
    Created: 2019-07-13
    License: GNU-All-Permissive

## Abstract ##

The idea is to provide a way to simplify peer exchange between crypto to
stabilise the coin value while keeping control over the money, without trusting
exchange services. To make it easy and fast for the client, it does just simple
exchange to other cryptocurrencies  like the "bureau of change" kind of exchange
(like like https://changelly.com/ , https://shapeshift.io/ or https://godex.io/)
that have advantages on normal exchange sites for people
that need to just exchange (not trade). Using any of those services requires
trust, so the idea is to use a web of trust connecting those sites using tor.

## Copyright ##

    Copying and distribution of this file, with or without modification,
    are permitted in any medium without royalty provided the copyright
    notice and this notice are preserved.  This file is offered as-is,
    without any warranty.

## Specification ##

The idea is to build an easy to deploy service to enable exchange between peers
on two different criptocurrencies, to setup the service the main requirement is
to setup two HD wallets on different criptocurrencies (the service needs to
have access to the private keys) and both wallets need to have some coins, as
it is insecure because those wallets will be connected to Internet the load
should be just enough for the system to work.

The application should start a tor hidden service and enable a server to allow
exchange between the coins, the exchange rate is defined by the current balance
on both wallets.

When visiting the site with a browser, you just need to type the amount you
want to exchange and the site display the amount that will be received. You
type the receiving address and receive a QR and address to receive the input.
Once the input is received, it automatically send the promised amount on the
other currency.

For that to work, the service needs to also be connected to both coin networks
to be able to react and send the money as soon as possible.

It will also provide an API, that will allow to connect to other peers (those
need to be whitelisted and don't need to be published), it will allow to
stabilize the currencies values after any exchange. This way, the amount in
play does not grow with big services, but with a trusted network of small
servers that automatically exchange between trusted pairs when any one do a
change to mitigate fluctuations.

## Motivation ##

Currently, exchange is mainly done on centralized trading platforms that need
to be trusted. FreeVision provides a way to exchange FairCoin to FIAT
currencies, but in no way simplify the exchange between crypto. Then this
network complement the view to remove the dependency on those exchanges.

## Rationale ##

The main advantage of this simple design is that it promotes distribution of
effort, risk and trust. Each server holds a minimum amount so anyone can join
the network with ease, clients only need to trust the node they are connecting
to, and that surely they already know, and running a server requires just a
simple application without a difficult setup.

It does not scale vertically with ease, because the protocol does not allow
that, but it scales horizontally with ease. Each peer can make a server with
just 0.03 BTC and connect with three trusted peers.

## Reference implementation ##

There is not reference implementation yet.

  [BIP]: https://github.com/bitcoin/bips/blob/master/bip-0001.mediawiki
  [FreeVision]: https://faircoin.co/
