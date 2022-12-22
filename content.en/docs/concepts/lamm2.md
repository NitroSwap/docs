---
title: LAMM 2
weight: 1
---

# LAMM 2

## Overview

The Loan Automated Market Maker (LAMM) is a smart contract that provides constant function liquidity for undercollateralized loans. In a LAMM, users can deposit margin and borrow leverage multiple times higher than their collateral value.

This is possible due to a couple reasons:

1. LAMM is the **custodian** of the margin trade
2. LAMM has visibility into the position's **profit and loss**

This design allows LAMMs to profit from a trader's self-interest, while conserving guarantees that would prevent the LAMM from incurring in loses beyond the trader's collateral.

Each LAMM points to an AMM pool where it obtains its trading liquidity, price oracles and other calculations like price impact and slippage. This is particularly important when trading long-tail assets, as they are susceptible to [oracle attacks](/docs/resources/reading-list/#oracle-vulnerabilities).

Below we attempt to provide a formal specification of the LAMM model.

## Liquidity

Each LAMM stores pooled reserves for a single asset, and provides loans for that asset maintaining the invariant that the reserves plus IOUs cannot decrease. The value of a LAMM pool can be expressed as

{{< katex display >}}
\Lambda = R(t) + D(t)
{{< /katex >}}

where {{< katex >}} R(t) {{< /katex >}} are the available token reserves and {{< katex >}} D(t) {{< /katex >}} are the token reserves taken out in open interest contracts. {{< katex >}} V {{< /katex >}} is denominated in units of the borrow token.

## Positions

When a position is built by a trader at time {{< katex >}} t {{< /katex >}}, the notional (position size) is taken to be

{{< katex display >}}
V(t) \equiv C(t) \cdot L
{{< /katex >}}

where {{< katex >}} C(t) {{< /katex >}} is the initial units of collateral backing the position and {{< katex >}} L {{< /katex >}} is the amount of initial leverage. {{< katex >}} V(t) {{< /katex >}} is in units of the asset being borrowed.

The initial open interest associated with this position is taken to be the number of contracts the trader has entered into

{{< katex display >}}
N(t) \equiv \frac{V(t)}{P(t)}
{{< /katex >}}

where {{< katex >}} P(t) {{< /katex >}} is the TWAP oracle value fetched at time {{< katex >}} t {{< /katex >}} when the position is built.

There are two calculations needed to determine the liquidation price and collateral takeover price of the position. They are

- Collateral Factor: {{< katex >}} CF(t) = \frac{C(t)}{V(t)} {{< /katex >}}
- Liquidation Factor: {{< katex >}} LF(t) = \frac{C(t) \cdot LT}{V(t)} {{< /katex >}}

where {{< katex >}} LT {{< /katex >}} is the liquidation threshold, a positive integer between {{< katex >}} 1 {{< /katex >}} and {{< katex >}} 2 {{< /katex >}}.

## Debt

Assuming a trader puts down an initial amount of collateral {{< katex >}} C(t) {{< /katex >}} and a leverage value of {{< katex >}} L {{< /katex >}}, the LAMM pool will track the open interest for the position and store a static reference to the debt

{{< katex display >}}
D(t) = V(t) - C(t) = C(t) \cdot (L - 1)
{{< /katex >}}

that the position "owes" to the protocol.

## Profit and Loss

The PnL offered to a position contract is linear in price, yet capped to allow for downside protection. A position contract receives

{{< katex display >}}
PnL(t_0, t_1) = \pm V(t_0) \cdot \frac{P(t_1) - P(t_0)}{P(t_0)} = \pm N(t_0) \cdot [P(t_1) - P(t_0)]
{{< /katex >}}

in {{< katex >}} PnL {{< /katex >}} where {{< katex >}} N(t) {{< /katex >}} is the open interest occupied by the position in units of number of position contracts, {{< katex >}} P(t_0) {{< /katex >}} is the entry price given to the position at time {{< katex >}} t_0 {{< /katex >}}, {{< katex >}} P(t_1) {{< /katex >}} is the exit price given to the position at time {{< katex >}} t_1 {{< /katex >}}. In the case of a long {{< katex >}} \pm = +1 {{< /katex >}} and {{< katex >}} \pm = -1 {{< /katex >}} in the case of a short. While {{< katex >}} \pm [\frac{P(t_1)}{P(t_0)} - 1] > LF(t_1) {{< /katex >}}, the position is able to experience a positive payoff function of {{< katex >}} e^x - 1{{< /katex >}}.

## Settlement (WIP)

Basically going to breakdown how to calcualte the collateral backing of a position at time t+r and then create payoffs scenarios:

1. close position profit

2. liquidation

<hr/>

Stuff to mention:

- Keepers: Explain how they're used to liquidate positions that are udner the water, explain how you used collateral factor, liquidation factor and liqudiation threshold
- Impermanent Loss: How do you calculate IL? Formally
- Game Theory?: pvp, coordination between marginpool and trader against an amm lp
- LAMM pools accrue value via trading fees and liquidations, and hence do not experience impermanent loss (IL).

{{< katex display >}}
f(x) = \int\_{-\infty}^\infty\hat f(\xi)\,e^{2 \pi i \xi x}\,d\xi
{{< /katex >}}

{{< mermaid class="text-center">}}

flowchart LR
WBTC-USDC-UNIV3-NITRO --> WBTC-USDC-UNIV3

{{< /mermaid >}}
