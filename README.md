# Filecoin Box

## Installation

> Note that this installation command will only work once the box is published (in the interim you can use `truffle unbox https://github.com/truffle-box/filecoin-box`).

```bash
$ truffle unbox filecoin
```

## Setup

### Running Filecoin Ganache

Once installed, you can run Filecoin Ganache with the following command (note that this requires NPM version 5.2 or above)

```bash
$ npx ganache filecoin
```

This creates 10 accounts, each loaded with 100 FIL, and display both their account addresses and associated private keys.

```bash
Available Accounts
==================
(0) t3rvcqmc5otc3sh3cngqg2ttzcu7ezpco466lbafzaoygxvnzsw7e7n2zbjwhiv5fdzhs6uxm2qckwt6lp5wga (100 FIL)
(1) t3s3la37547tijmoeiep7ktogws3tep2eqrralh7rhi2mpe46q574gceyy467356onblzvwf7ejlelo2rdsg4q (100 FIL)
(2) t3wk7a46e2dcqb7qxeuz2zq7wodwycdgtbgdpr37hhvelfilf5yvssg5xbsolgusqsumomtmtqhnobh4carhyq (100 FIL)
...
```

It also starts the Lotus and IPFS daemons running over `http` and `ws` respectively:

```bash
Lotus RPC listening on 127.0.0.1:7777
IPFS  RPC listening on 127.0.0.1:5001
```

### Optionally running the Filecoin Ganache GUI

An alternative to running Filecoin Ganache via the CLI is to use the Filecoin Ganche UI version. As per the screenshot below, this exposes all the core Filecoin elements as tabs which is particularly useful if you're just starting out.

![filecoin-ganache-ui](screenshots/filecoin-ganache-ui.png)

Filecoin Ganche UI can be installed via the following...

```
git clone https://github.com/trufflesuite/ganache
git checkout feat/filecoin-files
yarn
yarn start
```

### Running the Filecoin Network Explorer

> Note that these steps will be changing (merging branch into master / main, webpack, truffle plugin, etc).

```bash
$ git clone https://github.com/trufflesuite/filecoin-network-inspector
$ yarn
$ git checkout ganache-changes
$ yarn start
```

Assuming it's running correctly, you can open the Filecoin Network Explorer at the following: http://localhost:3000

### Running Ethereum Ganache

```bash
$ npx ganache ethereum
```

```bash
RPC Listening on 127.0.0.1:8545
```

## Creating Storage Deals for our Assets 

A [storage deals](https://docs.filecoin.io/store/lotus/store-data/#find-a-miner) is an agreement between a client and a storage miner to store some data in the network for a given duration. While in the case of the Filecoin mainnet, a deal must be secured with a miner, in Filecoin Ganache a deal is reached automatically.

To simulate this, open the Filecoin Network Explorer and navigate to the "Market" tab. From here you can select a file by clicking "Choose File" followed by "Upload to the Filecoin Network".

Alternatively, you can send the following curl request directly to the Lotus RPC. Note that the you'll need to update both the wallet address (`t3s3la3754...`) and CID (`QmZTR5bcpQ...`).

```bash
curl -X POST \
     -H 'Content-Type: application/json' \
     -d '{"jsonrpc":"2.0","id":0,"method":"Filecoin.ClientStartDeal","params":[{"Data":{"TransferType":"graphsync","Root":{"/":"QmZTR5bcpQD7cFgTorqxZDYaew1Wqgfbd2ud9QqGPAkK2V"},"PieceCid":null,"PieceSize":0},"Wallet":"t3s3la37547tijmoeiep7ktogws3tep2eqrralh7rhi2mpe46q574gceyy467356onblzvwf7ejlelo2rdsg4q","Miner":"t01000","EpochPrice":"2500","MinBlocksDuration":300}]}' \
     http://localhost:7777/rpc/v0
```

## Minting an NFT

In the example below, we've already created a deal for the 3 assets (metadata, thumbnail, and the original asset repsectively) that comprise our NFT. These are as follows, with their corresponding CIDs.

- metadata ([QmS4t7rFPxaaNriXvCmALr5GYRAtya5urrDaZgkfHutdCG](https://ipfs.io/ipfs/QmS4t7rFPxaaNriXvCmALr5GYRAtya5urrDaZgkfHutdCG))
- thumbnail - ([QmbAAMaGWpiSgmMWYTRtGsru382j6qTVQ4FDKX2cRTRso6](https://ipfs.io/ipfs/QmbAAMaGWpiSgmMWYTRtGsru382j6qTVQ4FDKX2cRTRso6))
- asset - ([QmUWFZQrJHfCVNHXVjjb2zeowVvH7dC6rKpbdHsTdnAgvP](https://ipfs.io/ipfs/QmUWFZQrJHfCVNHXVjjb2zeowVvH7dC6rKpbdHsTdnAgvP))

Assuming the local Ethereum Ganache node is running, you'll be able to open a console and mint a new NFT with the following steps. As the base URL is set to that of an IPFS gateway, we'll just need to pass in the CID to the asset metadata.

```bash
$ truffle console
truffle(development)> const gallery = await MyGallery.deployed()
truffle(development)> gallery.mint(accounts[0], "QmS4t7rFPxaaNriXvCmALr5GYRAtya5urrDaZgkfHutdCG")
```

In the above example the owner of the NFT is set (via `accounts[0]`) to that of the first account generated by the mnemonic. If we want to transfer it to a new owner, we'll be able to do so with the following.

### Transferring Ownership

```bash
truffle(development)> gallery.transferFrom(accounts[0], accounts[1], 1)
```

## Gallery UI

Coming soon.

## Resources

- [@ganache/filecoin](https://www.npmjs.com/package/@ganache/filecoin)
- [Demo](https://www.youtube.com/watch?v=mB1odTv6-3k&feature=youtu.be)