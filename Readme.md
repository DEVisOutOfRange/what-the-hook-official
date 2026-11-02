# What The Hook

What The Hook is a Uniswap v4 hook. When a pool that uses it falls out of line with its connected pools, the hook has an executor that closes the price difference in the same transaction. The revenue generated from closing the gap is returned back into the DeFi ecosystem.

## How it works

A trader swaps in a pool that uses the hook. Uniswap's PoolManager calls the hook before the swap (`beforeSwap`) and after it (`afterSwap`).

In either call, if the pool's price is out of line with its connected pools, the hook calls the executor, which brings the pool back in line with trades of its own.

- The profit is paid out in the same transaction: cashback to the trader, the protocol's share and, depending on the pool, a donation to its liquidity providers.
- The executor's trades are separate swaps in the same transaction, each with its own `Swap` event.
- Liquidity provider rewards can be seen in the pool manager's `Donate` events.

## On-chain proof

Each of these transactions holds the trader's swap, the arbitrage, and its payout. All are on [Robinhood Chain](https://robinhoodchain.blockscout.com).

### Hook v1

- **Transaction:** [`0xf636465e91555bc5981b95e856285a8e1ef464df07d7cf3ed3e6b7e4f7d29ebc`](https://robinhoodchain.blockscout.com/tx/0xf636465e91555bc5981b95e856285a8e1ef464df07d7cf3ed3e6b7e4f7d29ebc)
- **Arbitrage revenue:** 0.0644 WETH paid out, plus 0.00716 WETH as trader cashback.

### Hook v2

- **Transaction:** [`0xf96c2e0e3198ac5d8cba5e9b858151a605f16eaa9f0f5b7fd41401e0b7fbf948`](https://robinhoodchain.blockscout.com/tx/0xf96c2e0e3198ac5d8cba5e9b858151a605f16eaa9f0f5b7fd41401e0b7fbf948)
- **Arbitrage revenue:** $65 distributed from executing 2 arbitrages.

### Delta hook

- **Transaction:** [`0x0808cb8fab1aeba8e34cc0904afdca3ce6621564abe646e87c8593f74e37d7e8`](https://robinhoodchain.blockscout.com/tx/0x0808cb8fab1aeba8e34cc0904afdca3ce6621564abe646e87c8593f74e37d7e8)
- **Arbitrage revenue:** $35, a mix of USDG and WETH profit from 2 arbitrages.

### Hookr custom executor

- **Transaction:** [`0xf565159451be278ab2308eea9683b1eadad36b8b14046f9b28d2768913207f3a`](https://robinhoodchain.blockscout.com/tx/0xf565159451be278ab2308eea9683b1eadad36b8b14046f9b28d2768913207f3a)
- **Arbitrage revenue:** 2 arbitrages, $106 distributed in total.

## Why the executor's code is not public

The executor holds the logic that finds and closes these price gaps: which pools to route through, in what order, and with what size. That logic is the protocol's edge, so its source stays private.

What it does is public. Every trade it makes and every payout is in the same transaction as the swap that triggered it. Because the hook has no return-delta permission, the executor cannot alter the user's swap input or output.

## Links

- [Dashboard](https://www.whatthehook.io/dashboard)
- [Docs](https://www.whatthehook.io/docs)
- [X](https://x.com/whatthehookv4)

## Deployed addresses

Robinhood Chain. Addresses link to [Blockscout](https://robinhoodchain.blockscout.com).

### Hook v2

| Contract | Address |
| --- | --- |
| Hook | [`0x58A6eF29ddCdb064A72b550a78D84a8e065994C0`](https://robinhoodchain.blockscout.com/address/0x58A6eF29ddCdb064A72b550a78D84a8e065994C0) |
| Delta hook | [`0xB85138cbcAe0F7ddE777138392F44fD0C995d4C0`](https://robinhoodchain.blockscout.com/address/0xB85138cbcAe0F7ddE777138392F44fD0C995d4C0) |
| Executor | [`0xF3Cb5d18Efc11bb05a9CC86F3eaf763d6CB6Bb83`](https://robinhoodchain.blockscout.com/address/0xF3Cb5d18Efc11bb05a9CC86F3eaf763d6CB6Bb83) |
| Hookr executor | [`0x4B4c16681401454140a9dBca55e5c186BBE8D728`](https://robinhoodchain.blockscout.com/address/0x4B4c16681401454140a9dBca55e5c186BBE8D728) |

### Hook v1 (deprecated)

| Contract | Address |
| --- | --- |
| Hook | [`0xc52fc52698479E42F0dA9a8a75296EC3871454c0`](https://robinhoodchain.blockscout.com/address/0xc52fc52698479E42F0dA9a8a75296EC3871454c0) |
| Executor | [`0x2F4f6Dd51c0D8869852916Fceb869339Fe16aFB3`](https://robinhoodchain.blockscout.com/address/0x2F4f6Dd51c0D8869852916Fceb869339Fe16aFB3) |
