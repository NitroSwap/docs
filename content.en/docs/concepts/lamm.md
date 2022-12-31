---
title: LAMM
weight: 1
---

# LAMM

## Overview

The Loan Automated Market Maker (LAMM) is a smart contract that provides constant function liquidity for undercollateralized loans. In a LAMM, users can deposit margin and borrow leverage multiple times higher than their collateral value.

This is possible due to a couple reasons:

1. LAMM is the **custodian** of a user's trading position
2. LAMM has visibility into that position's **profit and loss**

This design allows LAMMs to benefit from a trader's self-interest, while conserving guarantees that would prevent the LAMM from incurring in loses beyond the trader's collateral.

Each LAMM points to an AMM pool where it obtains its trading liquidity, price oracles and other calculations like price impact and slippage. This is particularly important when trading long-tail assets, as they are susceptible to [oracle attacks](/docs/resources/reading-list/#oracle-vulnerabilities).

Below we attempt to provide a formal specification of the LAMM model.

## Configuration

To initialize a pool the following parameters must be provided:

| Variable                                       | Type    | Definition                                                                    |
| ---------------------------------------------- | ------- | ----------------------------------------------------------------------------- |
| {{< katex >}} baseAsset {{< /katex >}}         | address | The asset being used as collateral (eg.USDC)                                  |
| {{< katex >}} tradingAsset {{< /katex >}}      | address | The asset being traded (eg. UNI)                                              |
| {{< katex >}} priceOracle {{< /katex >}}       | address | The on-chain price feed denoting tradingAsset in terms of the baseAsset.      |
| {{< katex >}} maxLeverage {{< /katex >}}       | uint256 | Max amount of leverage allowed.                                               |
| {{< katex >}} \phi {{< /katex >}}              | uint256 | Fixed fee as a percentage of the position's value (where 100% equals `1e18`). |
| {{< katex >}} slippageTolerance {{< /katex >}} | uint256 | Max percentage of price slippage allowed during AMM swaps.                    |

## Liquidity

Let {{< katex >}} LP(t) {{< /katex >}} be the number of liquidity providers and {{< katex >}} r_i(t) {{< /katex >}} be the reserves deposited by the {{< katex >}} i^{th} {{< /katex >}} liquidity provider. Then the total available token reserves are

{{< katex display >}}
R(t) = \sum\_{i \in LP(t)} r_i(t).
{{< /katex >}}

Let {{< katex >}} M(t) {{< /katex >}} be the number of traders and {{< katex >}} d_i(t) {{< /katex >}} be the debt owed by the {{< katex >}} i^{th} {{< /katex >}} trader. Then the total debt owed by traders is

{{< katex display >}}
D(t) = \sum\_{i \in M(t)} d_i(t).
{{< /katex >}}

Each LAMM stores pooled reserves for a single asset, and provides loans for that asset maintaining the invariant that the reserves plus debt cannot decrease. The value of a LAMM pool can be expressed as

{{< katex display >}}
\Lambda(t) = R(t) + D(t)
{{< /katex >}}

where {{< katex >}} \Lambda {{< /katex >}} is denominated in units of the {{< katex >}} baseAsset {{< /katex >}}.

## Positions

Let {{< katex >}} c_i(t) {{< /katex >}} and {{< katex >}} l_i(t) {{< /katex >}} denote the {{< katex >}} i^{th} {{< /katex >}} investors collateral and leverage respectively. Then the value of a position built by the {{< katex >}} i^{th} {{< /katex >}} margin trader at time {{< katex >}} t {{< /katex >}} is

{{< katex display >}}
v_i(t) = c_i(t) \cdot l_i(t) \cdot (1 - \phi)
{{< /katex >}}

where the constant {{< katex >}} \phi {{< /katex >}} is a percentage fee that is deposited into the margin pool. For such a position the fee is calculated as

{{< katex display >}}
f_i(t) = \phi \cdot c_i(t) \cdot l_i(t)
{{< /katex >}}

such that the value of the position is simply the difference between the leveraged collateral and a fee

{{< katex display >}}
v_i(t) = c_i(t) \cdot l_i(t) - f_i(t)
{{< /katex >}}

The value of the position, also referred to as the notional, is denominated in units of the {{< katex >}} baseAsset {{< /katex >}}.

## State Transition

When a position is built it undergoes a state transition from the {{< katex >}} baseAsset {{< /katex >}} to the {{< katex >}} tradingAsset {{< /katex >}}. The value of the new position can be expressed as

{{< katex display >}}
x_i(t) = v_i(t) \cdot priceOracle(t) \cdot (1 - slippageTolerance)
{{< /katex >}}

where {{< katex >}} priceOracle(t) {{< /katex >}} is the number of {{< katex >}} tradingAsset {{< /katex >}} units exchanged for {{< katex >}} v_i(t) {{< /katex >}} at time {{< katex >}} t {{< /katex >}} after subtracting the max slippage {{< katex >}} slippageTolerance {{< /katex >}}.

## Debt

Assuming a trader puts down an initial amount of collateral {{< katex >}} c_i(t) {{< /katex >}} and a leverage value of {{< katex >}} l_i(t) {{< /katex >}}, the LAMM pool will track the open interest for the position and store a static reference to the debt

{{< katex display >}}
d_i(t) = v_i(t) - c_i(t) + f_i(t)
{{< /katex >}}

that the position "owes" to the protocol. This debt is denominated in units of the {{< katex >}} baseAsset {{< /katex >}}.

## Profit and Loss

A position's current PnL can be expressed as

{{< katex display >}}

PnL_i(t_1) = \frac{x_i(t)}{priceOracle(t_1)} - v_i(t)

{{< /katex >}}

## Settlement

While a position is active, it may be closed for profits or for loss as long as it's notional value after slippage and debt is larger than zero

{{< katex display >}}

\frac{x_i(t)}{priceOracle(t_1)} \cdot (1 - slippageTolerance) - d_i(t) > 0

{{< /katex >}}

A position may be liquidated at a price when the value of its collateral after losses and fees is less than or equal to zero {{< katex  >}}
c_i(t) - PnL(t_1) - f_i(t_1) \leq 0
{{< /katex >}}.

<hr/>
