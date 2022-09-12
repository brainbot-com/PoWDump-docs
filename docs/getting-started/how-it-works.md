---
sidebar_position: 1
title: How the swap works
---

## How the swap works

When proposing a swap on the website by entering an ETH PoW value to dump and a price , a transaction will be sent to
the `commit`function of our `EtherSwap` contract on PoW. This transaction
locks the ETH PoW and commits to a secret value only known to you (stored in your browser's localstorage), as well as to
a default lock duration of 10 minutes.

Counterparties interested in buying ETH PoW will pick up your swap offer, change the recipient of your swap on PoW to
their address using the `changeRecipient` function, and match the swap on the PoS chain by committing to the same secret
and values. You can then withdraw your ETH PoS on the PoS chain by revealing the secret value using the `claim`
function.

The counterparty matching your trade will need to claim its ETH PoW using the same secret value you revealed, finishing
the swap.

## Refunds

If no counterparty is interested in your trade, it will not be matched. After 10 minutes you will be able to get your
locked ETH PoW refunded. To do that, make sure you waited for the swap offer to expire and send a transaction to the
`refund` function of `EtherSwap` on PoW using the website.

This short 10 minute lock time enables you to shortly try to dump your ETH PoW again with a lower price. A too low lock
duration will not give enough time for counterparties to successfully match your swap.

## Fees

Fees are only paid ETH PoW buyers on PoS. The fee is locked when buyers commit to a swap on PoS: the buyer will need to
transfer `swap value + fee` to the contract when committing. As a result users will receive exactly the ETH PoS / ETH
PoW
values they committed to.

Fees are reimbursed if a swap expire.