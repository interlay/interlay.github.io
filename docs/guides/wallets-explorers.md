# Wallets and Explorers

Interacting with any blockchain requires to have a wallet.

At the end of this guide you will have:

- [x] [Installed the polkadot.js browser extension and created a Polkadot wallet](#polkadot-wallet)
- [x] [Installed and created a Bitcoin wallet](#bitcoin-wallet)
- [x] [Checked your account balances on Kintsugi / Interlay](#kintintr-balances)
- [x] [Testnet only: you will also have requested testnet KSM and testnet BTC](#testnet-faucets)

## Substrate Wallets

### Browser Extensions to Interact With Dapps

To interact with the Interlay/Kintsugi Dapp you will need a Substrate-compatible browser extension.

#### [Talisman](https://talisman.xyz/)

1. **Install** the Talisman extension in your browser: [https://talisman.xyz/download](https://talisman.xyz/download)
2. Follow the instructions to create a new account or import an existing one.

#### [polkadot.js](https://polkadot.js.org/extension/)

1. **Install** the polkadot-js extension in your browser: [https://polkadot.js.org/extension/](https://polkadot.js.org/extension/).

2. **Create account**. Go to the plus sign at the top of the extension and click on "create new account" and follow the instructions.

#### [SubWallet](https://subwallet.app/)

Guide: https://docs.subwallet.app/dapps-user-guide/interlay

### Web Wallets

<!-- tabs:start -->
#### **Interlay**

#### Polkadot.js Apps

Go to [https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi.interlay.io%2Fparachain#/explorer](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi.interlay.io%2Fparachain#/explorer)

#### Talisman

Go to [Talisman](https://app.talisman.xyz/)

#### **Kintsugi**

#### Polkadot.js Apps

Go to [https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/explorer](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/explorer)

#### Talisman

Go to [Talisman](https://app.talisman.xyz/)

#### **Testnet-Kintsugi**

#### Polkadot.js Apps

Go to [https://polkadot.js.org/apps/?rpc=wss%253A%252F%252Fapi-dev-kintsugi.interlay.io%252Fparachain#/explorer](https://polkadot.js.org/apps/?rpc=wss%253A%252F%252Fapi-dev-kintsugi.interlay.io%252Fparachain#/explorer)

#### **Testnet-Interlay**

#### Polkadot.js Apps

Go to [https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fstaging.interlay-dev.interlay.io%2Fparachain#/explorer](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fstaging.interlay-dev.interlay.io%2Fparachain#/explorer)

<!-- tabs:end -->


### Mobile Wallets

<!-- tabs:start -->

#### **Interlay**

#### [Nova wallet](https://novawallet.io/)

Supports Android and iOS.

#### [Math wallet](https://mathwallet.org/kintsugibtc-wallet/en/)

Supports Android and iOS. Web-wallet currently not working.

#### **Kintsugi**

#### [Nova wallet](https://novawallet.io/)

Supports Android and iOS.

#### [Math wallet](https://mathwallet.org/kintsugibtc-wallet/en/)

Supports Android and iOS. Web-wallet currently not working.

#### Coming soon

- [Fearless Wallet](https://fearlesswallet.io/)

<!-- tabs:end -->

### Create and Understand Accounts

#### Creating an Account

Follow the instructions in this [official Polkadot guide](https://support.polkadot.network/support/solutions/articles/65000098878-how-to-create-a-dot-account).

#### Understanding Accounts and Addresses

Polkadot uses [address formats](https://wiki.polkadot.network/docs/build-ss58-registryhttps://wiki.polkadot.network/docs/learn-accounts#address-format) for different chains. That way, you can use the same account and have unique addresses on each chain. By default, all addresses can be automatically converted between different chains.

For example, the `Alice` account can take the following formats:

- **Generic Substrate**: 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
- **Polkadot**: 15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5
- **Kusama**: HNZata7iMYWmk5RvZRTiAsSDhV8366zq2YGb3tLH5Upf74F
- **Kintsugi**: a3f1Q33MZ6B82T7rwQ1Ke1Qekzuxe8yRbfvRxkPh11jdsrTLR
- **Interlay**: wdCJ8CsZchTEfUP8Xz1eZKNRjW5cuYjJ9fh6pcZNXezsysBrJ

You can use the same account across different chains in the Polkadot ecosystem.

## Substrate Explorers

<!-- tabs:start -->

#### **Interlay**

#### Subscan

[interlay.subscan.io](https://interlay.subscan.io/)

#### Sub.id (Balances only)

Go to [sub.id](https://sub.id/#/) and enter your account.

#### Polkadot.js Apps

Go to [https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi.interlay.io%2Fparachain#/accounts](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi.interlay.io%2Fparachain#/accounts)

#### **Kintsugi**

#### Subscan

[kintsugi.subscan.io](https://kintsugi.subscan.io/)

#### Sub.id (Balances only)

Go to [sub.id](https://sub.id/#/) and enter your account.

#### Polkadot.js Apps

Go to [https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/accounts](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fapi-kusama.interlay.io%2Fparachain#/accounts)

#### **Testnet-Kintsugi**

#### Polkadot.js Apps

Go to [https://polkadot.js.org/apps/?rpc=wss%253A%252F%252Fapi-dev-kintsugi.interlay.io%252Fparachain#/accounts](https://polkadot.js.org/apps/?rpc=wss%253A%252F%252Fapi-dev-kintsugi.interlay.io%252Fparachain#/accounts)

#### **Testnet-Interlay**

#### Polkadot.js Apps

Go to [https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fstaging.interlay-dev.interlay.io%2Fparachain#/accounts](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fstaging.interlay-dev.interlay.io%2Fparachain#/accounts)

<!-- tabs:end -->

### Check Account Balance in Polkadot.js Developer Tab (Advanced)

<details>
<summary>
Click to expand
</summary>

More advanced users can use Polkadot.js developer tools to check the account balance.

#### View Balance in Developer > Chain state > Token

In the "Developer" tab select "Chain state".

![Go to chain state in Developer tab](../_assets/img/kintsugi/check-balance/3_chainstate.png)

Then, in the dropdown, select the "token" pallet.

![](../_assets/img/kintsugi/check-balance/4_select-tokens.png)

Select your Account in the top-level dropdown, and select "KINT" in the currency selector dropdown (see image).

Then click "+" in the top right of the form.

![Enter account and select KINT currency](../_assets/img/kintsugi/check-balance/5_enter-form.png)

You will now see your KINT balance as follows:

![View balance](../_assets/img/kintsugi/check-balance/6_view_balance.png)

?> Amounts are shown in **pico KINT** - the smallest currency unit on Kintsugi. To get to the actual KINT **divide by 1,000,000,000,000** (remove 12 decimal points). This is a Polkadot.js feature.

- **free** shows your **total KINT balance**
- **frozen** shows how much of your KINT are **still vesting**

?> `Available for transfer` = `free` - `frozen`
</details>


## Bitcoin Wallets

You can use basically **any** Bitcoin wallet to interact with the KBTC (on Kintsugi) and interBTC (on Interlay).


<!-- tabs:start -->

#### **Bitcoin Mainnet**
For a general overview of Bitcoin wallets you can consult [bitcoin.org's wallet selector](https://bitcoin.org/en/choose-your-wallet?step=5).

!> Attention: do **not** use mainnet Bitcoin wallets for Kintsugi / Interlay **testnet**

#### **Bitcoin Testnet**

For testnet, you can also pick one of the following Bitcoin wallets that support testnet accounts:

- **GreenAddress**: https://test.greenaddress.it/en/ (Android and IOS)
- **Bitcoin Testnet Wallet**: https://play.google.com/store/apps/details?id=de.schildbach.wallet_test (Android only)
- **Electrum**: https://electrum.org/#home (Linux, Windows, macOS and Android)
- **Bitpay**: https://bitpay.com/wallet/ (Linux; Guide here: https://support.bitpay.com/hc/en-us/articles/360015463612-How-to-Create-a-Testnet-Wallet)
<!-- tabs:end -->

#### Hardware Wallets

You can also use your hardware wallet. The following are tested and have Bitcoin mainnet and testnet support:

<!-- tabs:start -->

#### **Bitcoin Mainnet**

- **Ledger**: https://www.ledger.com/
- **Trezor**: https://trezor.io/

!> Attention: do **not** use mainnet Bitcoin wallets for Kintsugi / Interlay **testnet**

#### **Bitcoin Testnet**

- **Ledger**: https://www.ledger.com/ (Guide: https://coinguides.org/ledger-testnet/)
- **Trezor**: https://trezor.io/ (Guide: https://wiki.trezor.io/Bitcoin_testnet)

<!-- tabs:end -->

## Bitcoin Explorers

<!-- tabs:start -->

#### **Bitcoin Mainnet**

There are many Bitcoin Mainnet explorers. We find the following have the best user experience:

**[Blockstream](https://blockstream.info/)**
**[Blockchain.com](https://www.blockchain.com/explorer)**

#### **Bitcoin Testnet**

**[Blockstream Testnet Explorer](https://blockstream.info/testnet/)**

<!-- tabs:end -->

## Faucets (Testnet Only!)

To test, you will need testnet tokens. Please follow the steps below.

Attention:

- Testnet tokens have no economic value.
- Do not use real BTC or other mainnet assets on testnet!

### 1. Getting testnet KINT/INTR

You can get testnet KINT/INTR for transaction fees by clicking on the faucet link in the top bar.

?> These testnet KINT/INTR are only usable on testnet. They are **not real KINT/INTR and have no economic value**.

### 2. Getting testnet BTC

Make sure you have at least some testnet BTC in your wallet.
You can get testnet BTC by clicking on the faucet link in the top bar of testnet.interlay.io, or from one of the following faucets:

- [https://bitcoinfaucet.uo1.net/](https://bitcoinfaucet.uo1.net/)
- [https://coinfaucet.eu/en/btc-testnet/](https://coinfaucet.eu/en/btc-testnet/)
- [https://kuttler.eu/en/bitcoin/btc/faucet/](https://kuttler.eu/en/bitcoin/btc/faucet/)
- [https://tbtc.bitaps.com/](https://tbtc.bitaps.com/)

### 3. Getting testnet tokens for Vaults

If you are going to run a [Vault on testnet](/vault/installation?id=installing-the-vault-client), the Vault client will automatically request transaction fee tokens (KINT/INTR) and collateral tokens (KSM/DOT) on registration.

?> These testnet KINT/INTR/KSM/DOT are only usable on testnet. They are **not real KINT/INTR/KSM/DOT and have no economic value**.
