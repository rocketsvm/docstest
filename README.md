# Raptoreum Core wallet with CLI. Common usage

The goal of this guide is just to show common usage examples of managing Raptoreum Core wallet with command line interface on a single machine. Also there are many more advanced possibilities but you'll have to refer to `help` in executables.

## 1. Quick setup and blockchain sync

- Download latest version of the wallet for your OS from https://github.com/Raptor3um/raptoreum/releases/latest
- Unpack wallet from archive to any directory you want
  - To speed up synchronization process also download **bootstrap.zip** from [the same link](https://github.com/Raptor3um/raptoreum/releases/latest)
  - Unpack bootstrap to
    - Windows: `C:\Users\<user>\AppData\Roaming\RaptoreumCore\`
    - Linux: go figure :)
- Start Raptoreum Core server `raptoreumd -server`
- Wait till the synchronization is finished

You can check the state of sync with `raptoreum-cli getblockchaininfo`

Output example:
```
{
  "chain": "main",
  "blocks": 188609,
  "headers": 188609,
  "bestblockhash": "6321c44fe9118e9ec3ac1553848c3393dbcb26bc6e8a184efbd01846dc9b6f3c",
  "difficulty": 7.354045282476316,
  "mediantime": 1637421820,
  "verificationprogress": 0.9999714848995859,
  "chainwork": "0000000000000000000000000000000000000000000000000001294a57b22469",
  "pruned": false,
  "softforks": [
  ],
  "bip9_softforks": {
  }
}
```

When `"blocks"` height is close to the [current height of the chain](https://explorer.raptoreum.com/) and `"verificationprogress"` is close to `1`, your wallet is in sync.

## 2. Preparing wallet

While Raptoreum Core server is running you can create receiving address for your wallet:

`raptoreum-cli getnewaddress`

Output will be a string which represents one of your wallet addresses, e.g. `RLgmfGXfqPSowHEjo5TkS2A8MbU6gfU9KD`

You can create many receiving addresses for one wallet and assign labels for each of them:

`raptoreum-cli setaccount RLgmfGXfqPSowHEjo5TkS2A8MbU6gfU9KD "example wallet"`

Then you can check balance by address:

`raptoreum-cli getbalance RLgmfGXfqPSowHEjo5TkS2A8MbU6gfU9KD`

or by it's label:
 
`raptoreum-cli getbalance "example wallet"`

List of created labels and their balances is accessible with

`raptoreum-cli listaccounts`

Output:

```
{
  "": 0.00000000,
  "example wallet": 0.00000000,
  "main": 0.00000000
}
```

To know address of a given label:

`raptoreum-cli getaddressesbyaccount "main"`

## 3. Encrypting wallet

**ALWAYS** encrypt your wallets!!!

`raptoreum-cli encryptwallet "strong passphrase that you should store separately"`

Output:

`Wallet encrypted; Raptoreum Core server stopping, restart to run with encrypted wallet. The keypool has been flushed and a new HD seed was generated (if you are using HD). You need to make a new backup.`

Restart Core server after that.

## 4. Sending transactions

To send RTM from your encrypted wallet you'll have to temporarily unlock it first:

`raptoreum-cli walletpassphrase "strong passphrase that you should store separately" 60`

where `60` is an amount of seconds your wallet will be unlocked. During this amount of time you can send your RTMs to any address on the network:

`raptoreum-cli sendtoaddress "RCgaxsSdoBGtYoTv1mYqHoUC2FdR9nZB1k" 4.999`

Output will be the transaction ID. You may check it on [explorer](https://explorer.raptoreum.com/).
