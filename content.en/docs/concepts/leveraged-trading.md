---
title: Leveraged Trading
weight: 2
bookToc: false
---

# Trade

## Positions2

Let {{< katex >}} c_i(t) {{< /katex >}} and {{< katex >}} l_i(t) {{< /katex >}} denote the {{< katex >}} i^{th} {{< /katex >}} investors collateral and leverage respectively. Let the constant {{< katex >}} \phi {{< /katex >}} be the percentage of leveraged collateral that is deposited into the margin pool acting as a fee paid out to liquidity providers. Then the fee for a position built at time {{< katex >}} t {{< /katex >}} is calculated as

{{< katex display >}}
f_i(t) = \phi \cdot c_i(t) \cdot l_i(t)
{{< /katex >}}

such that the value of the position is simply the difference between the leveraged collateral and the fee

{{< katex display >}}
v_i(t) = c_i(t) \cdot l_i(t) - f_i(t).
{{< /katex >}}

The initial open interest associated with this position is taken to be the number of contracts the trader has entered into

{{< katex display >}}
n_i(t) \equiv \frac{v_i(t)}{P(t)}
{{< /katex >}}

where {{< katex >}} P(t) {{< /katex >}} is the TWAP oracle value fetched at time {{< katex >}} t {{< /katex >}} when the position is built.

There are two calculations needed to determine the liquidation price and collateral takeover price of the position. They are

- Collateral Factor: {{< katex >}} CF_i(t) = \frac{c_i(t)}{v_i(t)} {{< /katex >}}
- Liquidation Factor: {{< katex >}} LF_i(t) = \frac{c_i(t) \cdot LT}{v_i(t)} {{< /katex >}}

where {{< katex >}} LT {{< /katex >}} is the liquidation threshold, a positive integer between {{< katex >}} 1 {{< /katex >}} and {{< katex >}} 2 {{< /katex >}}.

## Profit and Loss

When a position is built a snapshot of the entire notional value {{< katex >}} v_i(t) {{< /katex >}} is taken using the {{< katex >}} priceOracle{{< /katex >}}

The PnL offered to a position contract is linear in price, yet capped to allow for downside protection. Suppose a position is built at time {{< katex >}} t_0 {{< /katex >}} then the profit (or loss) at time {{< katex >}} t_1 {{< /katex >}} is given by

{{< katex display >}}
PnL_i(t_1) = \pm v_i(t_0) \cdot \frac{P(t_1) - P(t_0)}{P(t_0)} = \pm n_i(t_0) \cdot [P(t_1) - P(t_0)]
{{< /katex >}}

in {{< katex >}} PnL {{< /katex >}} where {{< katex >}} n_i(t) {{< /katex >}} is the open interest occupied by the position in units of number of position contracts, {{< katex >}} P(t_0) {{< /katex >}} is the entry price given to the position at time {{< katex >}} t_0 {{< /katex >}}, {{< katex >}} P(t_1) {{< /katex >}} is the exit price given to the position at time {{< katex >}} t_1 {{< /katex >}}. In the case of a long {{< katex >}} \pm = +1 {{< /katex >}} and {{< katex >}} \pm = -1 {{< /katex >}} in the case of a short. While {{< katex >}} \pm [\frac{P(t_1)}{P(t_0)} - 1] > LF(t_1) {{< /katex >}}, the position is able to experience a positive payoff function of {{< katex >}} e^x - 1{{< /katex >}}.

## Overview

When a user trades using a LAMM they are forecasting the direction of a token's price. When this forecast is correct, the PnL is positive and the settlement is executed using the reserves of an AMM such as Uniswap or Curve. However, when the PnL is negative LAMMs can liquidate a user's margin. In some scenarios, liquidations may not happen in a timely manner and what would be considered as "bad debt" is expressed as impermanent loss (IL) for the LAMM LPs.

Quam iugulum regia simulacra, plus meruit humo pecorumque haesit, ab discedunt
dixit: ritu pharetramque. Exul Laurenti orantem modo, per densum missisque labor
manibus non colla unum, obiectat. Tu pervia collo, fessus quae Cretenque Myconon
crate! Tegumenque quae invisi sudore per vocari quaque plus ventis fluidos. Nodo
perque, fugisse pectora sorores.

## Collateral Factor (CF)

Vertitur mos ortu ramosam contudit dumque; placabat ac lumen. Coniunx Amoris
spatium poenamque cavernis Thebae Pleiadasque ponunt, rapiare cum quae parum
nimium rima.

## Long Position

Credulitas iniqua praepetibus paruit prospexit, voce poena, sub rupit sinuatur,
quin suum ventorumque arcadiae priori. Soporiferam erat formamque, fecit,
invergens, nymphae mutat fessas ait finge.

## Short Position

Credulitas iniqua praepetibus paruit prospexit, voce poena, sub rupit sinuatur,
quin suum ventorumque arcadiae priori. Soporiferam erat formamque, fecit,
invergens, nymphae mutat fessas ait finge.

## Closing Position

Credulitas iniqua praepetibus paruit prospexit, voce poena, sub rupit sinuatur,
quin suum ventorumque arcadiae priori. Soporiferam erat formamque, fecit,
invergens, nymphae mutat fessas ait finge.

## Liquidation

Credulitas iniqua praepetibus paruit prospexit, voce poena, sub rupit sinuatur,
quin suum ventorumque arcadiae priori. Soporiferam erat formamque, fecit,
invergens, nymphae mutat fessas ait finge.
