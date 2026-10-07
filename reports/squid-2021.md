# SQUID (2021) — Honeypot Rug Pull (The Name Everyone Knows)

**BSC · October–November 2021 · ~$3.4M drained**

This is our contrast case. It is the most famous rug pull in crypto — and the perfect demonstration of our core principle: **a big name does not mean safety; a complete evidence chain does.** Where public data is unavailable, we say so. We do not fabricate evidence.

---

## Executive Summary

### What the scam was

A token called SQUID launched on BSC in late October 2021, exploiting the popularity of the Netflix show *Squid Game*. It was a honeypot: the contract let buyers purchase tokens but blocked them from selling. The price ran from pennies to roughly **$2,861** in days, then collapsed to near zero in minutes when the developers cashed out — about **$3.4M** taken.

### How you would almost get caught

The chart was the trap. A price going straight up with no sell pressure looks like a rocket; it was actually a one-way door. Buyers could get in, but the contract's sell-block meant they could never get out until the developers chose to drain it.

### Three survival rules

1. **A token you can't sell is not an asset.** Test a small sell before you commit — if selling is blocked or "requires special conditions," walk away.
2. **Hype without substance is a honeypot's fuel.** A famous name, false partnerships (the team claimed a Netflix link that Netflix denied), and a disabled comment section are all warning signs.
3. **Anonymous team + a parabolic chart = leave.** No doxxed team, no audit, no real product — that combination has one ending.

---

## Technical Appendix

### 1. What is verifiable

| Fact | Status |
|---|---|
| Launch window: late October 2021, BSC | ✅ Public record |
| Peak price ~$2,861, then collapse to ~zero in minutes | ✅ Public record |
| ~$3.4M drained | ✅ Reported by Washington Post, TRM Labs, BBC, KuCoin |
| Honeypot / sell-block mechanism | ✅ Confirmed in contract analysis |
| Primary contract (upgradeable proxy) | `0x87230146E138d3F296a9a77e497A2A83012e9Bc5` |
| Hidden privilege address (`_sir`) | `0x6BdB3b0fd9F39427a07b8ab33Bac32Db67EB4E38` |
| Sell-block transaction | `0x5b82e96f…555041` |
| Staking-pool drain transaction | `0xf7c9d0e5…2cb6af` |
| Exit wallet (→ Tornado) | `0x34400280…f0aa` |

### 2. The honest gap

The **liquidity-removal transaction hash** for the SQUID 2021 drain is **not publicly disclosed** in the sources we reviewed. Media reports state the "$3.4M taken" figure without attaching the specific removal transaction. We were unable to independently verify a single authoritative liquidity-removal hash for this event.

**Data unavailable — not fabricated.** We do not invent a hash to make the report look complete. This gap is exactly why the SQUIDGAME (2023) case is our flagship: there, the full drain chain is verifiable end-to-end.

### 3. Why this matters

Compare the two cases side by side:

| | SQUIDGAME (2023) | SQUID (2021) |
|---|---|---|
| Name recognition | Low (copycat) | Very high (the original) |
| Liquidity-removal hash | ✅ Verifiable | ❌ Not publicly disclosed |
| Full evidence chain | ✅ Complete | ⚠️ Partial |

A bigger name did not produce a stronger case — it produced a fuzzier one. Our rule: **we report what is verifiable, and we say when it is not.**

---

*This report is based on public on-chain data and reputable third-party coverage. The noted evidence gap is disclosed intentionally.*
