# NEAR-CLP Usage

Our smart contracts are implemented in Rust. To investigate our API, please refer to the exported Rust crate documentation. All public functions are exported through a blockchain interface.
Currently, the crate documentation it's checked-in the github repository. To see it in a friendly way you need to clone this repository and open the `contract/target/doc/near_clp/index.html` file in your web browser.

You can call any `NEARClp` public function using RPC or the [near-cli](https://github.com/near/near-cli) tool. Example:

```
near view beta-1.nearswap.testnet price_near_to_token_out '{"token": "gold.nearswap.testnet", "tokens_out": "100000000000000000000"}' --accountId me.testnet
```


## Web application

### Features

The interface is hosted and maintained on [Skynet](https://siasky.net/) as to be a decentralized application (DApp), and currently supports swapping between and among NEAR and NEP-21 tokens. You can connect to your NEAR wallet and Ethereum wallet to view the available tokens and their balances. ERC-20 tokens on the Ethereum blockchain can be bridged to NEP-21 tokens on NEAR via the [Rainbow Bridge](https://near-examples.github.io/erc20-to-nep21/).

#### Making a swap

First, select the currencies you would like to trade to and from. In the input box labeled _I want_, enter your desired amount and the application will return the amount you need to put in. Ensure that the amount does not exceed your balance of the input currency or you will not be able to swap. Next, click the swap button and you will be redirected to a page to confirm the transfer, which will include a small fee in NEAR. Finally, you will be redirected back to the application and will be able to see your updated balance if the swap was successful.

Note that when converting from a NEP-21 token, a small deposit of NEAR is required to allow the contract access to your funds. When a NEP-21 token is selected, it will display its allowance, and you will be prompted to approve access to your tokens before a swap can be made.

### Limitations

It is currently only recommended to use the interface for testing purposes until more error handling is added and edge cases are tested. The CLI tool can be used in conjunction with the web interface to add or remove liquidity to pools or to call methods directly.

### Planned

- Liquidity provision and withdrawal  via the _Pool_ page.
- Integrated ERC-20 to NEP-21 conversion directly on our dapp (without going to the rainbow bridge web application).
- More rigorous error handling.
- Creating new pools (currently this is available through near-cli or nearswap-cli).
- Price oracles.

## nearswap-cli

We implemented a dedicated [CLI tool](https://github.com/luciotato/near-clp-beta-cli/) to directly interact with our smart contract. Please refer to the project page to see setup and usage instructions.

By using nearswap-cli, liquidity providers can operate the contract from the command line: create a new pool, config the pool, add and withdraw liquidity, retrieve pool status and prices.

The nearswap-cli also can interact with the NEP-21 contracts, by using the command `inc_allowance` you can call directly to a NEP21 token contract from the nearswap-cli

Example:

```
> nearswap list_pools

View call: beta-1.nearswap.testnet.list_pools()
[ 'gold.nearswap.testnet', 'usd.nearswap.testnet', [length]: 2 ]

```
```
> nearswap pool_info { token:gold.nearswap.testnet }

View call: beta-1.nearswap.testnet.pool_info({"token":"gold.nearswap.testnet"})
{
  ynear: "12998486894407298000000000",
  reserve: "221800000030020300000000",
  total_shares: "1000000000022852100000"
}
```

Increase allowance on a NEP21 token contract
```
> nearswap inc_allowance usd24.nearswap.testnet 500
```

Please go to [nearswap CLI tool](https://github.com/luciotato/near-clp-beta-cli/) for full instructions.
