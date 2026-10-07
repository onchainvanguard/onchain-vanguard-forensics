# SQUIDGAME (copycat) — Rug Pull Forensics Report

**BSC · November 24, 2023 · ~$55,800 drained**

This is our flagship case. It shows the full standard of evidence we deliver: every step of the drain is backed by a transaction hash you can open on BscScan and verify yourself.

---

## Executive Summary

### What the scam was

A token called SQUIDGAME launched on BNB Smart Chain, riding the name of the 2021 Squid Game rug pull to lure buyers. Within a day, its entire liquidity pool was pulled — roughly **237.31 WBNB (~$55,800)**&#8203; swapped out — and the price dropped 100%. This is a **hard rug**: the liquidity was removed, not just dumped.

### How you would almost get caught

The contract was built to look safe. Its constructor called `transferOwnership(address(0))`, which makes the token appear *renounced* — the classic "the dev can't touch it now" signal buyers check for. It was a lie. The apparent renunciation removed nothing, and every single transfer was secretly routed through a hidden helper contract that decided the real balances.

### Three survival rules

1. **"Renounced ownership" means nothing until you read the code.** A fake renunciation is a known trick — verify there is no function that still controls transfers.
2. **Every transfer should be self-contained.** If a transfer calls out to an external contract to decide a balance, that external contract controls your money.
3. **A token with no sell pressure and a big green candle is a target, not an opportunity.** Copycat names of already-rugged tokens are a red flag on arrival.

---

## Technical Appendix

### 1. The drain (verifiable on BscScan)

| Step | Evidence |
|---|---|
| Liquidity removal | `0x11ad1095fac841c1c2c9d09f1f01871ac6f41fbc33da76d141c5fb3dd05cf7a0` |
| Removed from pool | **237.31 WBNB** (~$55.8k) |
| Insider payout, 11h 19m before the sale | `0x53ea5e3c740613112aedbab8b87eba9b868934be119db1064745c700c2dea000` ($21,274 to the developer) |
| Independent security alert | PeckShield `#PeckShieldAlert` (Nov 24, 2023) |

The same event is recorded in the **De.Fi REKT database** as a rug pull of $55,539, describing 237.31 WBNB removed from the liquidity pool, proceeds moved to another EOA and spread across several addresses.

- Deployer: `0x6dA430E6…91CDbd`
- Scammer (seller): `0x2cFe2D07…577e61`

### 2. The backdoor — fake renunciation + hidden helper (reverse-engineered)

The contract's `constructor` does the following:

```solidity
constructor(uint256 amount) {
    _mint(msg.sender, amount);
    _transferOwnership(address(0));
}
```

**The trap:**&#8203; inside `_mint`, the first use of the `amount` argument is:

```solidity
INX inx = INX(address(uint160(amount + 912932389747193719218)));
```

The `amount` plus a hard-coded constant becomes the address of a **hidden helper contract**. The next line overwrites `amount` with a fixed 10 billion supply for the deployer.

Then `_transferOwnership(address(0))` writes to a standalone `_owner` variable and emits `OwnershipTransferred` — but **no function in the contract is gated on `_owner`**, so the apparent renunciation removes nothing.

Every transfer (including a router sell via `transferFrom`) runs:

```solidity
uint256 curBalance = balanceoF(_balances[from], from);
require(curBalance >= amount, "ERC20: transfer amount exceeds balance");
_balances[from] = curBalance - amount;
```

The `balanceoF` call invokes `inx.dissort(ba, sender)` on the helper. **Balances are therefore controlled by the helper's logic, not by the token's own accounting.** This is a more subtle trap than a classic honeypot: there is no visible `blacklist` or `mint` function — the control lives outside the token contract entirely.

### 3. Reproduction

To confirm the drain yourself:

1. Open BscScan and paste the liquidity-removal hash `0x11ad1095…5cf7a0`.
2. Verify the token transfer amount and the WBNB received.
3. Confirm the price chart for SQUIDGAME on the same date shows a 100% drop.
4. Cross-check the PeckShield alert and the De.Fi REKT entry for the same event.

---

*This report is based on public on-chain data and third-party security disclosures. Every hash is verifiable on BscScan.*
