---
layout: default
title: Cardano SL Wallet Frontend
permalink: /technical/wallet-frontend/
group: technical
visible: true
---
[//]: # (Reviewed at cd26fb28eb48f893a4ca2d045a10da19c211b807)

# Cardano SL Wallet Frontend

When developing Cardano SL, the need arose for a UI from which users could access
their funds, send and receive transactions, and perform other tasks related to
managing a personal cryptocurrency wallet. The Daedalus wallet is the Cardano's
solution to these necessities.

Currently, it allows a user to use their ADA in the aforementioned actions, and providing
support for other currencies is planned for the near future — as is the exchange
between different currencies, both digital and not.

## Building `daedalus-client-api`

To run `daedalus-client-api` locally, you have to start the `wallet-api` of [`cardano-sl`](https://github.com/input-output-hk/cardano-sl/) as follows. Make sure that you are in the root folder of `cardano-sl`.

~~~bash
util-scripts/build-daedalus-bridge.sh
~~~

## Running and testing `daedalus-client-api`

In order to see `daedalus-client-api` in action, first run a local Cardano network:

~~~bash
# run tmux in another window
tmux
# launch nodes
util-scripts/start-dev.sh
~~~

By default, this should launch Cardano network consisting of 3 nodes talking to each other. `WALLET_TEST=1` tells the launcher script to run `wallet-api` with one node. This one node running `wallet-api` will behave the same as Daedalus wallet that is run in production.


With `wallet-api` running, you can run `daedalus-client-api` locally as follows.
Please note that [npm](https://www.npmjs.com/) is required to build `daedalus-client-api`.


Now we can try using the client API with [nodejs](https://nodejs.org/):

~~~bash
$ node
> var api = require('../output/Daedalus.ClientApi')
undefined
> api
{ applyUpdate: [Function],
  blockchainSlotDuration: [Function],
  changeWalletPass: [Function],
  deleteAccount: [Function],
  deleteWallet: [Function],
  generateMnemonic: [Function: generateMnemonic],
  getHistory: [Function],
  getLocale: [Function],
  getWalletAccounts: [Function],
  getAccount: [Function],
  getWallet: [Function],
  getWallets: [Function],
  getAccounts: [Function],
  importWallet: [Function],
  isValidAddress: [Function],
  isValidMnemonic: [Function],
  isValidPaperVendRedemptionKey: [Function: isValidPaperVendRedemptionKey],
  isValidRedemptionKey: [Function: isValidRedemptionKey],
  newWAddress: [Function],
  newPayment: [Function],
  newPaymentExtended: [Function],
  newAccount: [Function],
  newWallet: [Function],
  nextUpdate: [Function],
  notify: [Function],
  redeemAda: [Function],
  redeemAdaPaperVend: [Function],
  renameWalletSet: [Function],
  reportInit: [Function],
  restoreWallet: [Function],
  searchAccountHistory: [Function],
  searchHistory: [Function],
  syncProgress: [Function],
  systemVersion: [Function],
  testReset: [Function],
  updateLocale: [Function],
  updateTransaction: [Function],
  updateAccount: [Function] }
~~~

This will load and show all functions that can be run from from this library to interact with the wallet. For example, to fetch all available wallets, we can do:

~~~bash
> api.getAccounts().then(console.log).catch(console.log)
Promise { <pending> }
> [ { cwMeta: { cwName: 'Initial wallet' },
    cwId: '1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648',
    cwAmount: { getCCoin: '50000' },
    cwAccounts: [ [Object] ] } ]
~~~

Note: `daedalus-client-api` is not optimized/compressed. This is will be a job for Daedalus.

## Wallet Frontend API usage scenario

This is an example session that shows how the user/client can work with the Daedalus-bridge library.

~~~bash
var api = require('../output/Daedalus.ClientApi')
~~~

### Clear all data (DEVELOPMENT)

~~~bash
> api.testReset().then(console.log).catch(console.log)
Promise { <pending> }
> {}
~~~

### Get all wallets

~~~bash
>  api.getWallets().then(console.log).catch(console.log)
Promise { <pending> }
> []
~~~

### New wallet

~~~bash
>  api.newWallet('test', 'CWANormal', 0, 'transfer uniform grunt excess six veteran vintage warm confirm vote nephew allow', 'pass').then(console.log).catch(console.log)
Promise { <pending> }
> { cwsWalletsNumber: 0,
  cwsWSetMeta: { cwsUnit: 0, cwsName: 'test', cwsAssurance: 'CWANormal' },
  cwsPassphraseLU: 1495542169.630769,
  cwsId: '1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW',
  cwsHasPassphrase: true,
  cwsAmount: { getCCoin: '0' } }
~~~

### Get (existing) wallet

~~~bash
> api.getWallet('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW').then(console.log).catch(console.log)
Promise { <pending> }
> { cwsWalletsNumber: 0,
  cwsWSetMeta: { cwsUnit: 0, cwsName: 'test', cwsAssurance: 'CWANormal' },
  cwsPassphraseLU: 1495542169.630769,
  cwsId: '1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW',
  cwsHasPassphrase: true,
  cwsAmount: { getCCoin: '0' } }
~~~

### Get all wallets

~~~bash

> api.getWallets().then(console.log).catch(console.log)
Promise { <pending> }
> [ { cwsWalletsNumber: 0,
    cwsWSetMeta: { cwsUnit: 0, cwsName: 'test', cwsAssurance: 'CWANormal' },
    cwsPassphraseLU: 1495542169.630769,
    cwsId: '1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW',
    cwsHasPassphrase: true,
    cwsAmount: { getCCoin: '0' } },
  { cwsWalletsNumber: 1,
    cwsWSetMeta:
     { cwsUnit: 0,
       cwsName: 'Precreated wallet full of money',
       cwsAssurance: 'CWANormal' },
    cwsPassphraseLU: 1495541138.013531,
    cwsId: '1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f',
    cwsHasPassphrase: false,
    cwsAmount: { getCCoin: '50000' } } ]
~~~

### Change passphrase

~~~bash
> api.changeWalletPass('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW', 'pass', 'pass2').then(console.log).catch(console.log)
Promise { <pending> }
> {}

> api.changeWalletPass('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW', 'pass', 'pass2').then(console.log).catch(console.log)
Promise { <pending> }
> Error: ServerError: Pos.Wallet.Web.Error.Internal "Invalid old passphrase given"

> api.changeWalletPass('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW', 'pass2', 'pass').then(console.log).catch(console.log)
Promise { <pending> }
> {}
~~~

### Change wallet name

~~~bash
>  api.renameWalletSet('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW', 'testing').then(console.log).catch(console.log)
Promise { <pending> }
> { cwsWalletsNumber: 0,
  cwsWSetMeta: { cwsUnit: 0, cwsName: 'testing', cwsAssurance: 'CWANormal' },
  cwsPassphraseLU: 1495542169.630769,
  cwsId: '1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW',
  cwsHasPassphrase: true,
  cwsAmount: { getCCoin: '0' } }

>  api.renameWalletSet('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW', 'test').then(console.log).catch(console.log)
Promise { <pending> }
> { cwsWalletsNumber: 0,
  cwsWSetMeta: { cwsUnit: 0, cwsName: 'test', cwsAssurance: 'CWANormal' },
  cwsPassphraseLU: 1495542169.630769,
  cwsId: '1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW',
  cwsHasPassphrase: true,
  cwsAmount: { getCCoin: '0' } }
~~~

### Restore wallet

~~~bash
>  api.restoreWallet('test', 'CWANormal', 0, 'transfer uniform grunt excess six veteran vintage warm confirm vote nephew allow', 'pass').then(console.log).catch(console.log)
Promise { <pending> }
> Error: ServerError: Pos.Wallet.Web.Error.RequestError "Wallet set with that mnemonics already exists"


>  api.deleteWallet('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW').then(console.log).catch(console.log)
Promise { <pending> }
> {}


>  api.restoreWallet('test', 'CWANormal', 0, 'transfer uniform grunt excess six veteran vintage warm confirm vote nephew allow', 'pass').then(console.log).catch(console.log)
Promise { <pending> }
> { cwsWalletsNumber: 0,
  cwsWSetMeta: { cwsUnit: 0, cwsName: 'test', cwsAssurance: 'CWANormal' },
  cwsPassphraseLU: 1495542169.630769,
  cwsId: '1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW',
  cwsHasPassphrase: true,
  cwsAmount: { getCCoin: '0' } }
~~~

### Delete a wallet (shown above)

~~~bash
>  api.deleteWallet('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW').then(console.log).catch(console.log)
Promise { <pending> }
> {}
~~~


### Import wallet
If you are in development mode, make sure to create keys with ``.

~~~bash
> api.importWallet('/home/akegalj/projects/serokell/cardano-sl/keys/2.key.hd', '').then(console.log).catch(console.log)
Promise { <pending> }
> { cwsWalletsNumber: 0,
  cwsWSetMeta:
   { cwsUnit: 0,
     cwsName: 'Genesis wallet',
     cwsAssurance: 'CWANormal' },
  cwsPassphraseLU: 1495545014.377285,
  cwsId: '1feqWtoyaxFyvKQFWo46vHSc7urynGaRELQE62T74Y3RBs8',
  cwsHasPassphrase: false,
  cwsAmount: { getCCoin: '0' } }
~~~

### Get wallets

~~~bash
> api.getAccounts().then(console.log).catch(console.log)
Promise { <pending> }
> [ { cwMeta: { cwName: 'Genesis wallet' },
    cwId: '1feqWtoyaxFyvKQFWo46vHSc7urynGaRELQE62T74Y3RBs8@2147483648',
    cwAmount: { getCCoin: '50000' },
    cwAccounts: [ [Object] ] },
  { cwMeta: { cwName: 'Initial wallet' },
    cwId: '1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648',
    cwAmount: { getCCoin: '50000' },
    cwAccounts: [ [Object] ] } ]
~~~

### Get wallet

~~~bash
> api.getAccount('1feqWtoyaxFyvKQFWo46vHSc7urynGaRELQE62T74Y3RBs8@2147483648').then(console.log).catch(console.log)
Promise { <pending> }
> { cwMeta: { cwName: 'Genesis wallet' },
  cwId: '1feqWtoyaxFyvKQFWo46vHSc7urynGaRELQE62T74Y3RBs8@2147483648',
  cwAmount: { getCCoin: '50000' },
  cwAccounts:
   [ { caId: '19FLnEFfkaLsZqBqYHjPmCypZNHNZ7SBfMsntKgspqA96F18s6eeDy5GYjHmwXSECG6jRqWh9qqEAicpEXrNhpb8PuRNVL',
       caAmount: [Object] } ] }
~~~

### Get accounts from a specific wallet

~~~bash
> api.getWalletAccounts('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f').then(console.log).catch(console.log)
Promise { <pending> }
> [ { cwMeta: { cwName: 'Initial account' },
    cwId: '1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648',
    cwAmount: { getCCoin: '50000' },
    cwAccounts: [ [Object] ] } ]
~~~

### Create a new account

~~~bash
> api.newAccount('1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW', 'trips', 'pass').then(console.log).catch(console.log)
Promise { <pending> }
> { cwMeta: { cwName: 'trips' },
  cwId: '1fjgSiJKbzJGMsHouX9HDtKai9cmvPzoTfrmYGiFjHpeDhW@3190108780',
  cwAmount: { getCCoin: '0' },
  cwAccounts:
   [ { caId: '19M3DbeepAzN6xzSSErL8pk1JQA8oFkgE9L6LZfKXMiNpoPDjfDpJjWa3Jis1oCZVGMo1pM8tio2wifuhDPWzwCWS6sZfX',
       caAmount: [Object] } ] }
~~~

### Delete an account

~~~bash
>  api.deleteAccount('1feqWtoyaxFyvKQFWo46vHSc7urynGaRELQE62T74Y3RBs8@2147483648').then(console.log).catch(console.log)
Promise { <pending> }
> {}
~~~

### Update an account

~~~bash
> api.updateAccount('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648','CWTPersonal','ADA','Initial account','CWANormal',0).then(console.log)
Promise { <pending> }
> { cwMeta: { cwName: 'CWTPersonal' },
  cwId: '1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648',
  cwAmount: { getCCoin: '50000' },
  cwAccounts:
   [ { caId: '19Fv6JWbdLXRXqew721u2GEarEwc8rcfpAqsriRFPameyCkQLHsNDKQRpwsM7W1M587CiswPuY27cj7RUvNXcZWgTbPByq',
       caAmount: [Object] } ] }
~~~

### Create a new account

~~~bash
> api.newWAddress('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', '').then(console.log).catch(console.log)
Promise { <pending> }
> { caId: '19N52o4RrzEo6AxRzawAkbuMtnqPjrgat1USDMaRQG3uK46b7bNrpxMSLgd1sxvPUPFbGnmj9Kmj2Fb8H5W5Ez7g6voZMy',
  caAmount: { getCCoin: '0' } }
~~~

### Is the address valid

~~~bash
> api.isValidAddress('1feqWtoyaxFyvKQFWo46vHSc7urynGaRELQE62T74Y3RBs8').then(console.log).catch(console.log)
Promise { <pending> }
> true

> api.isValidAddress('19MxMbcEskurDMdVX1h32Fi94Nojxp1gvwMYbDziZoPjGmJdssagaugyCqUUJVySKBdA1DUHbpYmQd6yTeFQqfrWWKx9gs').then(console.log).catch(console.log)
Promise { <pending> }
> true

> api.isValidAddress('19MxMbcEskurDMdVX1h32Fi94Nojxp1gvwMYbDziZoPjGmJdssagaugyCqUUJVySKBdA1DUHbpYmQd6yTeFQqfrWWKx9g').then(console.log).catch(console.log)
Promise { <pending> }
> false

> api.isValidAddress('1feqWtoyaxFyvKQFWo46vHSc7urynGaRELQE62T74Y3RBs9').then(console.log).catch(console.log)
Promise { <pending> }
> false
~~~

### New payment

~~~bash
> api.newPayment('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', '19MxMbcEskurDMdVX1h32Fi94Nojxp1gvwMYbDziZoPjGmJdssagaugyCqUUJVySKBdA1DUHbpYmQd6yTeFQqfrWWKx9gs', 1, '').then(console.log).catch(console.log)
Promise { <pending> }
> { ctOutputAddrs:
   [ '19G8L5LAih7ExGFSyhLGLSYxbCEB3VgHB1XA2qn8T4mHnUvDiFH77sGhXTVZLvig8Hi1SrJ7oLGaxBzSxoxZzMVLbwPrQM',
     '19MxMbcEskurDMdVX1h32Fi94Nojxp1gvwMYbDziZoPjGmJdssagaugyCqUUJVySKBdA1DUHbpYmQd6yTeFQqfrWWKx9gs' ],
  ctMeta: { ctmTitle: '', ctmDescription: '', ctmDate: 1495545716.778353 },
  ctInputAddrs: [ '19Fv6JWbdLXRXqew721u2GEarEwc8rcfpAqsriRFPameyCkQLHsNDKQRpwsM7W1M587CiswPuY27cj7RUvNXcZWgTbPByq' ],
  ctId: '84a54480afd3e232ef7a158ba4548973dbfffd214ff38faf978776fc1f85e209',
  ctConfirmations: 0,
  ctAmount: { getCCoin: '50000' } }
~~~

### New payment, extra data

~~~bash
> api.newPaymentExtended('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', '19MxMbcEskurDMdVX1h32Fi94Nojxp1gvwMYbDziZoPjGmJdssagaugyCqUUJVySKBdA1DUHbpYmQd6yTeFQqfrWWKx9gs', 1, 'Title', 'Descrption', '').then(console.log).catch(console.log)
Promise { <pending> }
> { ctOutputAddrs:
   [ '19HLdDRHWZp3hf8YS2bSjovYND1HeCfC3zRvGV1FsnwNj7GkQ5af1zJTxcjY7yKi6vKdZfQAxJMQxfjwe8tDSimxpwSjSn',
     '19MxMbcEskurDMdVX1h32Fi94Nojxp1gvwMYbDziZoPjGmJdssagaugyCqUUJVySKBdA1DUHbpYmQd6yTeFQqfrWWKx9gs' ],
  ctMeta:
   { ctmTitle: 'Title',
     ctmDescription: 'Descrption',
     ctmDate: 1495546265.710291 },
  ctInputAddrs: [ '19JfqFuwNSkbCRPYzNppNZodz6PzP3FrSyN694yxf3WXE1hufexxYwfSpXPHS2J8SxdCU8vMCYdbHwWtdLg5RFTqQxTygR' ],
  ctId: 'e6b9c5b27806970f25eadeaa30d1ccd8330eb0ebfcf07713f0a64e9d234caf8a',
  ctConfirmations: 0,
  ctAmount: { getCCoin: '49999' } }
~~~

### Update transaction meta-data

~~~bash
> api.updateTransaction('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', 'cc7576fef33a4a60865f9149792fa7359f44eca6745aeb1ba751185bab9bd7ac', 'Manager task', 'Managing people and other stuff', 1494935150.0468155).then(console.log).catch(console.log)
Promise { <pending> }
> {}
~~~

### Get history

~~~bash
>  api.getHistory('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', 0, 10).then(console.log).catch(console.log)
Promise { <pending> }
> [ [ { ctOutputAddrs: [Object],
      ctMeta: [Object],
      ctInputAddrs: [Object],
      ctId: '84a54480afd3e232ef7a158ba4548973dbfffd214ff38faf978776fc1f85e209',
      ctConfirmations: 0,
      ctAmount: [Object] },
    { ctOutputAddrs: [Object],
      ctMeta: [Object],
      ctInputAddrs: [Object],
      ctId: 'c8579e1ff7535ec5fe894c63b6c3b4bb4d71ee7782eca3a3d09b09b122d8134e',
      ctConfirmations: 0,
      ctAmount: [Object] },
    { ctOutputAddrs: [Object],
      ctMeta: [Object],
      ctInputAddrs: [Object],
      ctId: 'e6b9c5b27806970f25eadeaa30d1ccd8330eb0ebfcf07713f0a64e9d234caf8a',
      ctConfirmations: 0,
      ctAmount: [Object] } ],
  3 ]
~~~

### Search history

~~~bash
>  api.searchHistory('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', 'Title', 0, 10).then(console.log).catch(console.log)
Promise { <pending> }
> [ [ { ctOutputAddrs: [Object],
      ctMeta: [Object],
      ctInputAddrs: [Object],
      ctId: 'e6b9c5b27806970f25eadeaa30d1ccd8330eb0ebfcf07713f0a64e9d234caf8a',
      ctConfirmations: 0,
      ctAmount: [Object] } ],
  3 ]
~~~

### Search account history

~~~bash
> api.searchAccountHistory('1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', '19GfdoC3ytim4rsTXRMp5At6Bmt512XkcbUwGV69jqWvuRhU5HS5gNAbQ6JpUDavDiKRNMb9iyp6vKUCdJiaKLJdhmcQN9', '', 0, 10).then(console.log).catch(console.log)
Promise { <pending> }
> [ [ { ctOutputAddrs: [Object],
      ctMeta: [Object],
      ctInputAddrs: [Object],
      ctId: '84a54480afd3e232ef7a158ba4548973dbfffd214ff38faf978776fc1f85e209',
      ctConfirmations: 0,
      ctAmount: [Object] },
    { ctOutputAddrs: [Object],
      ctMeta: [Object],
      ctInputAddrs: [Object],
      ctId: 'c8579e1ff7535ec5fe894c63b6c3b4bb4d71ee7782eca3a3d09b09b122d8134e',
      ctConfirmations: 0,
      ctAmount: [Object] },
    { ctOutputAddrs: [Object],
      ctMeta: [Object],
      ctInputAddrs: [Object],
      ctId: 'e6b9c5b27806970f25eadeaa30d1ccd8330eb0ebfcf07713f0a64e9d234caf8a',
      ctConfirmations: 0,
      ctAmount: [Object] } ],
  3 ]
~~~

### Get next update

~~~bash
> api.nextUpdate().then(console.log).catch(console.log)
Promise { <pending> }
> Error: ServerError: Pos.Wallet.Web.Error.Internal "No updates available"
~~~

### Apply the update

~~~bash
> api.applyUpdate().then(console.log).catch(console.log)
Promise { <pending> }
> {}
~~~


### Redeem ADA
TODO: this endpoint wasn't verified yet! Need to be tested with genesis block prepared for redeeming!

~~~bash
> api.redeemAda('lwIF94R9AYRwBy0BkVVpLhwtsG3CmqDvMahlQr3xKEY=', '1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', '').then(console.log).catch(console.log)
Promise { <pending> }
> Error: ServerError: Pos.Wallet.Web.Error.RequestError "Cannot send redemption transaction: Failed to prepare inputs!"
~~~

### Redeem ADA papervend
TODO: this endpoint wasn't verified yet! Need to be tested with genesis block prepared for redeeming!

~~~bash
> api.redeemAdaPaperVend('CUUCxbH5pPc6RC1dvJKetJPVTG7X4WKPmBsoeZMbiQok', 'nurse indicate parent few rib awake glue enact opera', '1gCC3J43QAZo3fZiUTuyfYyT8sydFJHdhPnFFmckXL7mV3f@2147483648', '').then(console.log).catch(console.log)
Promise { <pending> }
> Error: ServerError: Pos.Wallet.Web.Error.RequestError "Cannot send redemption transaction: Failed to prepare inputs!"
~~~

### Redeem valid key

~~~bash
> api.isValidRedemptionKey('lwIF94R9AYRwBy0BkVVpLhwtsG3CmqDvMahlQr3xKEY=')
true
~~~

### Redeem valid paper vend

~~~bash
> api.isValidPaperVendRedemptionKey('lwIF94R9AYRwBy0BkVVpLhwtsG3CmqDvMahlQr3xKEY=')
false
> api.isValidPaperVendRedemptionKey('CUUCxbH5pPc6RC1dvJKetJPVTG7X4WKPmBsoeZMbiQok')
true
~~~

### Reporting

~~~bash
> api.reportInit(1, 1).then(console.log).catch(console.log)
Promise { <pending> }
> {}
~~~

### Settings blockchain slot duration

~~~bash
> api.blockchainSlotDuration().then(console.log).catch(console.log)
Promise { <pending> }
> 7000
~~~

### Settings system version

~~~bash
> api.systemVersion().then(console.log).catch(console.log)
Promise { <pending> }
> { svNumber: 0, svAppName: { getApplicationName: 'cardano-sl' } }
~~~

### Settings sync progress

~~~bash
> api.syncProgress().then(console.log).catch(console.log)
Promise { <pending> }
> { _spPeers: 0,
  _spNetworkCD: null,
  _spLocalCD: { getChainDifficulty: 4 } }
~~~

### Generate mnemonic

~~~bash
> api.generateMnemonic()
'obtain divide top receive purchase shuffle opinion circle future spare athlete quantum'
~~~

### Is valid mnemonic

~~~bash
> api.isValidMnemonic(12, 'obtain divide top receive purchase shuffle opinion circle future spare athlete quantum')
true
~~~

### Notify websockets

We can test the websockets with a small utility application(`npm install -g wscat`):
~~~bash
> wscat -c ws://127.0.0.1:8090                              

connected (press CTRL+C to quit)

< {"tag":"ConnectionOpened"}

< {"tag":"NetworkDifficultyChanged","contents":{"getChainDifficulty":1}}
< {"tag":"LocalDifficultyChanged","contents":{"getChainDifficulty":1}}
< {"tag":"NetworkDifficultyChanged","contents":{"getChainDifficulty":2}}
< {"tag":"LocalDifficultyChanged","contents":{"getChainDifficulty":2}}
< {"tag":"NetworkDifficultyChanged","contents":{"getChainDifficulty":3}}
< {"tag":"LocalDifficultyChanged","contents":{"getChainDifficulty":3}}
< {"tag":"NetworkDifficultyChanged","contents":{"getChainDifficulty":4}}
< {"tag":"LocalDifficultyChanged","contents":{"getChainDifficulty":4}}
~~~      

We should be seeing the same changes manually from here:
~~~bash
curl http://localhost:8090/api/settings/sync/progress
~~~                                                                         

Accound should be renamed into address. Please see an issue [CSM-249](https://issues.serokell.io/issue/CSM-249) for details.

### Wallet events

Aside from these HTTP endpoints, there is one unidirectional websocket channel
opened from server to client, the `notify` endpoint.

This channel serves as a notification system so
that Daedalus UI can be informed about events. Currently supported events are:

* `LocalDifficultyChanged` - local blockchain height,
* `NetworkDifficultyChanged` - global blockchain height,
* `UpdateAvailable` - new system update available,
* `ConnectedPeersChanged` - number of peers connected to the node changed,
* `ConnectionOpened` - websocket connection opened,
* `ConnectionClosed` - websocket connection closed.

As this channel is unidirectional, any message sent to the channel from the
client will be ignored.