---
title: LAMM
weight: 1
---

# LAMM

## Overview

The Loan Automated Market Maker (LAMM) is a novel mechanism for managing liquidity for undercollateralized loans. In a LAMM, users can deposit margin and borrow leverage at a fraction of their collateral value. This is useful for traders who wish to long/short [ERC-20](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/) tokens on decentralized exchanges (DEXs).

This is possible due to a couple reasons:

1. LAMM is the **custodian** of the margin position
2. LAMM has visibility into the position's **profit and loss**

By design, LAMMs exist on top of AMMs. Each LAMM points to an AMM pool where it obtains its trading liquidity, price oracles and other calculations like price impact and slippage. This is particularly important when trading long-tail assets, as they are susceptible to oracle attacks as seen with [Rari](https://cmichel.io/replaying-ethereum-hacks-rari-fuse-vusd-price-manipulation/) and more recently, [Aave](https://blockworks.co/news/aave-curve-bad-debt).

When a user trades using a LAMM they are forecasting the direction of a token's price. When this forecast is correct, the PnL is positive and the settlement is executed using the reserves of an AMM such as Uniswap or Curve. However, when the PnL is negative LAMMs can liquidate a user's margin. In some scenarios, liquidations may not happen in a timely manner and what would be considered as "bad debt" is expressed as impermanent loss (IL) for the LAMM LPs.

## Loan Automated Market Maker (LAMM)

We formalize the constant loan liquidity of the LAMM model and analyze its properties.

{{< mermaid class="text-center">}}

flowchart LR
WBTC-USDC-UNIV3-NITRO --> WBTC-USDC-UNIV3

{{< /mermaid >}}

Stuff to mention:

- Configruation: We are tieing each margin pool to an amm pool
- Liquidity: Mention how reserves work, how there are 2 tokens per pair
- Keepers: Explain how they're used to liquidate positions that are udner the water, explain how you used collateral factor, liquidation factor and liqudiation threshold
- Impermanent Loss: How do you calculate IL? Formally
- Game Theory?: pvp, coordination between marginpool and trader against an amm lp
