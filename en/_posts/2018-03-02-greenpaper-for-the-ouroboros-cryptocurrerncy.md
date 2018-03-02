---
---

I were reading a lot about cryptocurrencies, and as it always happens some new
and revolutionary idea that I cannot implement occurs to me. Let me share this
here.

The idea is to build a cryptocurrency based on a blockchain that eat itself,
let me explain. The idea is to use CryptoNote in which each payment is received
in a brand new address so we are not really interested in old transactions that
involves addresses that have no more money and will never have.

This way, each address can only appear in the blockchain twice, the first in the
block they receive coins and the second when the address make a payment sending
coins. In this blockchain, all transactions should empty the originating
address, but it is an standard practice anyway.

This way addresses will have a limited lifetime of let say 1 year, after that
old blocks are _eaten_ and _digested_ by the blockchain once the limit is reached.

## Eat and digestion process explained.

Once 364\*24\*20 (174720) have been created, the next block start the eating
process, the block then generate new coins based on the first block, it also
stores the hash of the eaten block. It also contains new transactions but will
not accept transactions from addresses on the blockchain being eaten.

The next block will eat and digest the second block, and this process will
continue, the last 20*24 (480) blocks should be stored for reference, but after
that old blocks can and should be trashed. Those 480 blocks (one day), should
allow to verify the chain correctness.

## What is the purpose

The idea is that it makes sure that old accounts are removed, forgotten private
keys are removed, but coins can be recovered, making it possible to know more
about the current usage of the coin. It also promotes coin usage instead of
accumulation.
