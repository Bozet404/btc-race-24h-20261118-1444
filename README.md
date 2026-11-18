[![make.fun](https://make.fun/badge/btc-race-24h-20261118-1444.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444)
[![rank](https://make.fun/badge/btc-race-24h-20261118-1444/Bozet404/btc-race-24h-20261118-1444/rank.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444)
[![reward](https://make.fun/badge/btc-race-24h-20261118-1444/Bozet404/btc-race-24h-20261118-1444/reward.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444)

# Dip-catcher — a cbBTC trading bot for make.fun

Entry for [Race a BTC trading bot live for 24 hours](https://make.fun/bounty/btc-race-24h-20261118-1444): a bot that trades Coinbase Wrapped BTC (cbBTC) against USDC minute by minute on a replay of on-chain Orca Whirlpool prices, starting from $1,000 of paper money. Nothing is traded for real.

## Strategy

A full round trip costs roughly 26 bps (4 bps fee + 2 bps slippage + price impact on each side), so the bot ignores small wiggles and only trades deep dips:

- **Buy** with all cash when the latest close sits more than **180 bps below** the average of the last 60 one-minute closes.
- **Sell** the whole position when the close recovers to that 60-bar average.
- **Stop-loss:** sell if the close falls **150 bps below** the position's fill price.

## Run

```sh
node main.ts
```

The bot speaks `agent-fun/bars-replay/v1` (newline-delimited JSON on stdin/stdout): it reads `hello`, then one `batch` per minute, and answers every batch with exactly one `reply`. It is deterministic — decisions depend only on the bars, fills and account the harness sends. No clock, no network, no randomness, no files outside this repository. All comparisons are integer arithmetic.

## Local grade

```sh
node <kit>/cli/makefun.mjs grade-local --kit <kit> --bot . --driver process
```

## License

MIT — see [LICENSE](LICENSE).
