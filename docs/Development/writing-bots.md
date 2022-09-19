---
sidebar_position: 2
title: Buy bots
---

As already touched in the [Buyer section](/getting-started/buyer) the PoWDump website doesn't have a UI for
buyers of PoW ETH. Instead, interested parties are encouraged to write a trading bot to match orders.

### Writing your own bots - gotchas

Be extremely careful with the swap `_swapEndTimestamp`! Always check the `_swapEndTimestamp`
of the user's swap order and set your `_swapEndTimestamp` to an earlier value. Since you are not dealing with an order book
and no high frequency trading is taking place you should account for the human factor.
A user needs enough time to react to your match. You should make sure that you
give the user enough time to reveal (at least few minutes) and that you also have enough time to submit your reveal
transaction once you have the user's proof.

:::warning recipientChangeLockDuration
If a swap `_swapEndTimestamp` is set to a value longer than 20 minutes you should pay attention to
the `recipientChangeLockDuration` contract parameter.

The contracts were deployed with `recipientChangeLockDuration` set to 20 minutes. This means that if you add yourself as
the recipient of a swap you have a maximum of 20 minutes to submit the reveal transaction. If you don't submit the
reveal transaction within that timeframe you risk someone else changing the recipient of the swap to their address and
claiming the ETH PoW (if they know the secret). Because of this, if you engage in a swap that expires in more than 20 
minutes make sure that you set your swap expiration to __20 minutes minus the time you need for the reveal transaction__ 
on the PoW chain.

Failure to account for this can result in you losing the locked ETH PoW and the user leaving with your ETH PoS.
:::
