---
sidebar_position: 2
title: Swap Contract
---

Under the hood the PoWDump DApp relies on a smart-contract to reliably & securely
dump the user's PoW ETH.

The source code of the contract can be
found [here](https://github.com/brainbot-com/PoWDump/blob/main/packages/contracts/contracts/EtherSwap.sol).

The contract was deployed by anonymous users on the Ethereum network with different constructor arguments:

First contract deployment:

```
Arg [0] : _recipientChangeLockDuration (uint64): 1200
Arg [1] : _feeRecipient (address): 0xc5bf0223730816f4c5057c13eab5ae99988f7795
Arg [2] : _feePerMillion (uint64): 1000
```

The contract can be viewed on
etherscan [0xdc8c99452dfcc0ed0574cfaa52f5cb90c8f8131c](https://etherscan.io/address/0xdc8c99452dfcc0ed0574cfaa52f5cb90c8f8131c#code)

Second contract deployment:

```
Arg [0] : _recipientChangeLockDuration (uint64): 1200
Arg [1] : _feeRecipient (address): 0xc5bf0223730816f4c5057c13eab5ae99988f7795
Arg [2] : _feePerMillion (uint64): 0
```

The contract can be viewed on
etherscan [0xdC7650c7562E53115eDB02cCC7bf67FB9F79cfF4](https://etherscan.io/address/0xdC7650c7562E53115eDB02cCC7bf67FB9F79cfF4#code)

The contracts are available under the same addresses on the PoW chain:
[https://www.oklink.com/en/ethw/address/0xdc8c99452dfcc0ed0574cfaa52f5cb90c8f8131c](https://www.oklink.com/en/ethw/address/0xdc8c99452dfcc0ed0574cfaa52f5cb90c8f8131c)
[https://www.oklink.com/en/ethw/address/0xdC7650c7562E53115eDB02cCC7bf67FB9F79cfF4](https://www.oklink.com/en/ethw/address/0xdC7650c7562E53115eDB02cCC7bf67FB9F79cfF4
)

Unfortunately we couldn't validate the source code of the contracts in the OKLink explorer, since it doesn't provide an
option to pass constructor arguments in the verification process.(note: If you know of a solution to this problem,
please let us
know.)

