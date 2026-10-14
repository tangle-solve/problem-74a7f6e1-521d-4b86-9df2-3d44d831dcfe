<!-- solve-problem:74a7f6e1-521d-4b86-9df2-3d44d831dcfe -->
# Unsecured credit under costly pseudonyms

A [Tangle Solve problem](https://solve.tangle.tools/problems/74a7f6e1-521d-4b86-9df2-3d44d831dcfe).

## What is the problem?

Consider a repeated lending game on a open, permissionless network (blockchain). Borrowers are pseudonymous addresses; there is no legal enforcement of debt. Formalize as follows.

- **Stage game:** a lender extends an unsecured loan of size *L* to borrower identity *i*. The borrower repays *L(1+r)* or defaults, keeping the proceeds.
- **Enforcement:** the only sanction is exclusion of identity *i* from future credit (reputation). The borrower may discard *i* and re-enter under a fresh identity at cost *c* ≥ 0 (wallet creation, on-chain deposits, sybil-resistant attestations, gas, Know-Your-Customer friction, or stake locked per identity).
- **Uncertainty:** the continuation value of being a credit-worthy identity is stochastic — with probability per period *q* the network/reputation system loses value (protocol obsolescence, migration, collapse), so the discounted value of reputation is a random variable, not a known constant.

**Question:** characterize the maximum feasible loan size *L\**(c, δ, q, r, ...)* such that repayment is an equilibrium strategy — i.e., the incentive constraint "one-shot default gain < discounted continuation value of reputation, net of identity-replacement cost" holds despite the uncertainty. Then determine whether unsecured (undercollateralized) credit is possible at all for realistic *c* and *q*, or whether an impossibility result holds: feasible *L\** collapses to zero whenever reputation value is sufficiently uncertain or pseudonym cost sufficiently low.

Ambiguity note: the census record does not pin down the exact stochastic model of reputation value (i.i.d. death probability vs. Markov chain vs. ambiguous belief). The natural reading is an i.i.d. per-period survival probability; a solver should state their choice explicitly.

## Why does it matter?

Essentially all DeFi lending today (Compound, Aave, Maker) is overcollateralized precisely because this problem is unsolved; solving it would unlock capital-efficient undercollateralized lending on permissionless infrastructure — the missing piece for on-chain mortgages, invoice factoring, and unsecured consumer credit. It also determines how much value protocols must burn on sybil resistance (costly identity) versus collateral. A clean impossibility theorem would tell designers to stop looking for reputation tricks and pursue alternatives (undercollateralization via social/DAO enforcement, legal wrappers, or escrow).

## What's already known?

- **Cheap-pseudonym baseline:** Friedman & Resnick (2001) showed that when identities are free (*c* = 0), reputation systems support only trivial cooperation — social cost is high and repeated-game incentives fail. This is the classic whitewashing result the problem extends.
- **Community enforcement:** Kandori (1992) and subsequent contagion-based results show cooperation can survive player turnover only under strong informational conditions; a blockchain gives full public histories, so the binding constraint is identity turnover, not information.
- **DeFi security literature:** Werner et al.'s SoK (2021/2022) catalogs DeFi mechanisms and explicitly notes lending is collateral-dominated; Daian et al. (2019) and Gudgeon et al. (2020) analyze manipulation/liquidation risks but not the pseudonym problem.
- **Limited commitment:** the economics literature on borrowing constraints with limited commitment (Kehoe–Levine-style) establishes the collateralization intuition but assumes persistent, non-replicable identity — the exact assumption that fails here.
- **Gap:** no published characterization of feasible loan size as a function of (i) positive identity cost *c* and (ii) stochastic reputation survival. The 2021–2022 DeFi security work treats identity cost as infinite (addresses can't be locked) or zero; the intermediate, uncertain regime is open.

## What would count as solving it?

1. **Impossibility (suggested type):** a theorem that for any *c* below a threshold *ĉ(q, δ)*, no symmetric equilibrium supports any positive unsecured loan *L > 0*, where *ĉ → ∞* as survival probability (1−*q*) → 0 for fixed δ. Give tight constants.
2. **Possibility/construction:** a mechanism (staked identity, credit tiering, bonding) and an equilibrium analysis showing positive feasible *L\** with an explicit formula, plus the matching upper bound showing *L\** cannot be improved within the mechanism class.
3. **Empirical validation (alternative):** on-chain analysis of actual identity-replacement costs and default-whitewashing rates (e.g., borrowers abandoning addresses after defaults on Aave/Compound) estimating realized *c* and *q*, showing which regime we are in.

A referee accepts: a fully specified game, an explicit incentive constraint, and either the theorem or the calibrated empirical bound.

## How to start

1. Read Friedman & Resnick (2001) first — the entire problem is the *c* > 0, stochastic-survival refinement of their model. Write down the borrower's deviation payoff: default gain *L* minus identity restart cost *c*, versus continuation value *V(reputation)* which is now an expectation over survival.
2. Model reputation value as geometric with per-period survival 1−*q*: continuation value ∝ δ(1−*q*)/(1−δ(1−*q*)). The incentive constraint *L ≤ δV(1−q)... + c* gives a candidate *L\**(c, δ, q)* — this one-line derivation is your baseline; the hard part is robustness (borrower heterogeneity, unobservable *q*, lender learning).
3. Read the SoK (Werner et al., Section on lending) to ground the mechanism-design side: what actually constitutes identity cost on-chain today (verify current values — EOA creation is nearly free; ENS/staking/attestations are the realistic cost carriers).
4. Main obstacle: lenders cannot observe whether a fresh address is a whitewashed old one, so you need either a poisoning/contagion argument (Kandori-style) or an assumption about identity-issuance; both have known fragilities.

## Key references

- J. W. Werner, D. Perez, et al., "SoK: Decentralized Finance (DeFi)," arXiv:2101.08778 (2021/2022). — Systematizes DeFi including lending-protocol design and its security assumptions; documents collateral dominance. https://arxiv.org/abs/2101.08778
- E. J. Friedman and P. Resnick, "The Social Cost of Cheap Pseudonyms," *Journal of Economics & Management Strategy* 10(2), 2001. — Proves that free identity replacement destroys reputation-based cooperation and quantifies the resulting efficiency loss; the direct ancestor of this problem. (verify venue details)
- M. Kandori, "Social Norms and Community Enforcement," *Review of Economic Studies* 59(1), 1992. — Foundational result on sustaining cooperation under anonymous player matching via contagious punishments.
- P. Daian, S. Goldfeder, et al., "Flash Boys 2.0: Frontrunning, Transaction Reordering, and Consensus Instability in Decentralized Exchanges," *IEEE S&P / FC 2019*, arXiv:1904.05234. — Shows adversarial behavior and priority manipulation on-chain; relevant background for the 2021–2022 DeFi security line. https://arxiv.org/abs/1904.05234
- L. Gudgeon, P. Perez, S. Harz, B. Livshits, et al., "DeFi Protocols for Loanable Funds," *FC 2020*. — Analyzes the overcollateralized debt structure of Compound/Aave-style protocols and liquidation dynamics.
- T. Kehoe and D. Levine, "Debt-Constrained Asset Markets," *Review of Economic Studies* 60(4), 1993. — Canonical limited-commitment lending model with persistent identity; the benchmark that fails under pseudonymity.

## Warnings

- Do not assume identity cost *c* is exogenous and constant — on most chains creating an address costs ~0; any interesting result needs *c* to come from a mechanism you design or a measured friction, and then strategic behavior toward that mechanism must be analyzed.
- The "uncertainty in reputation's future value" is the census record's vaguest clause; a common mistake is conflating borrower discounting δ with protocol-survival probability. Keep them separate.
- Watch for folk-theorem triviality: with arbitrary punishment schemes you can always "support" cooperation; the meaningful question is feasibility under *minimal* or *credible* punishments (Nash reversion / exclusion only), so state the equilibrium concept.
- Empirical estimates of whitewashing on-chain are confounded by airdrop farming and address churn for unrelated reasons.

---
*`BCGT-000345` · flagged for review*

## Acceptance guidance

1. **Impossibility (suggested type):** a theorem that for any *c* below a threshold *ĉ(q, δ)*, no symmetric equilibrium supports any positive unsecured loan *L > 0*, where *ĉ → ∞* as survival probability (1−*q*) → 0 for fixed δ. Give tight constants.
2. **Possibility/construction:** a mechanism (staked identity, credit tiering, bonding) and an equilibrium analysis showing positive feasible *L\** with an explicit formula, plus the matching upper bound showing *L\** cannot be improved within the mechanism class.
3. **Empirical validation (alternative):** on-chain analysis of actual identity-replacement costs and default-whitewashing rates (e.g., borrowers abandoning addresses after defaults on Aave/Compound) estimating realized *c* and *q*, showing which regime we are in.

A referee accepts: a fully specified game, an explicit incentive constraint, and either the theorem or the calibrated empirical bound.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/74a7f6e1-521d-4b86-9df2-3d44d831dcfe?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000345.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000345)
