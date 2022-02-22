# Wallets

Interacting with any blockchain requires to have a wallet.

At the end of this guide you will have:

- [x] [Installed the polkadot.js browser extension and created a Polkadot wallet](#polkadot-wallet)
- [x] [Installed and created a Bitcoin wallet](#bitcoin-wallet)
- [x] [Testnet only: you will also have requested testnet KSM and testnet BTC](#testnet-faucets)

## Polkadot Wallet

### 1. Installing a Polkadot wallet (polkadot-js browser extension)

You will need the polkadot-js browser extension. Install the polkadot-js extension in your browser: [https://polkadot.js.org/extension/](https://polkadot.js.org/extension/).

### 2. Create a new account

Go to the plus sign at the top of the extension and click on "Create new account" and follow the instructions.

### 3. Accounts and addresses

Polkadot uses [address formats](https://wiki.polkadot.network/docs/build-ss58-registryhttps://wiki.polkadot.network/docs/learn-accounts#address-format) for different chains. That way, you can use the same account and have unique addresses on each chain. By default, all addresses can be automatically converted between different chains.

For example, the `Alice` account can take the following formats:

- **Generic Substrate**: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
- **Polkadot**: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5
- **Kusama**: HNZata7iMYWmk5RvZRTiAsSDhV8366zq2YGb3tLH5Upf74F
- **Kintsugi**: a3f1Q33MZ6B82T7rwQ1Ke1Qekzuxe8yRbfvRxkPh11jdsrTLR

You can easily use the same account across different chains in the Polkadot ecosystem.

## Bitcoin Wallet

### 1. Installing a Bitcoin wallet

For a general overview of Bitcoin wallets you can consult [bitcoin.org's wallet selector](https://bitcoin.org/en/choose-your-wallet?step=5).

You can also pick one of the following Bitcoin wallets below. The selected wallets below are compatible with Bitcoin testnet and will make it easy to test interBTC.

- **GreenAddress**: https://test.greenaddress.it/en/ (Android and IOS)
- **Bitcoin Testnet Wallet**: https://play.google.com/store/apps/details?id=de.schildbach.wallet_test (Android only)
- **Electrum**: https://electrum.org/#home (Linux, Windows, macOS and Android)
- **Bitpay**: https://bitpay.com/wallet/ (Linux; Guide here: https://support.bitpay.com/hc/en-us/articles/360015463612-How-to-Create-a-Testnet-Wallet)

#### Hardware Wallets

You can also use your hardware wallet. The following are tested and have Bitcoin mainnet and testnet support:

- **Ledger**: https://www.ledger.com/ (Guide: https://coinguides.org/ledger-testnet/)
- **Trezor**: https://trezor.io/ (Guide: https://wiki.trezor.io/Bitcoin_testnet)


### 2. Creating a Bitcoin wallet

Depending on the Bitcoin wallet installed in step 1, follow the instructions in the wallet to create an account.

## Testnet Faucets

### 1. Getting testnet DOT

You can get testnet KSM by clicking on the faucet link in the top bar of https://testnet.interlay.io.

?> These testnet KSM are only usable on testnet. They are **not real KSM and have no economic value**.

### 2. Getting testnet BTC

Make sure you have at least some tBTC in your wallet.
You can get testnet BTC by clicking on the faucet link in the top bar of testnet.interlay.io, or from one of the following faucets:

- [https://testnet-faucet.mempool.co/](https://testnet-faucet.mempool.co/)
- [https://bitcoinfaucet.uo1.net/](https://bitcoinfaucet.uo1.net/)
- [https://coinfaucet.eu/en/btc-testnet/](https://coinfaucet.eu/en/btc-testnet/)
- [https://kuttler.eu/en/bitcoin/btc/faucet/](https://kuttler.eu/en/bitcoin/btc/faucet/)
- [https://tbtc.bitaps.com/](https://tbtc.bitaps.com/)

### Great! You're now ready to [ mint your first interBTC](/guides/issue)
