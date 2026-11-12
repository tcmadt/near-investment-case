# Demo script

Tobé Madu. 12 Nov 2026.

Judges can run this two ways.

**Tabletop.** Use the fixtures in this file. Do the arithmetic. Apply the allowlist. No chain calls.

**Prototype.** Run the same steps against the build. Any step that did not hit a chain must log `source: fixture` or `source: deterministic`. A missing label is a fail, because a fixture must not look like a live fill.

The $500,000 book in the spec is illustrative. This script uses a three-asset test policy so the amounts close. Live funds, if any, stay on testnet or under a separate cap the builder prints at the start. Do not send the illustrative size.

Reference NAV in every fixture: $500,000. Daily cap: $10,000. Drift trigger: absolute gap of 5 percentage points or more. Dust: under $100, no trade. Trade choice: the most overweight sleeve funds the most underweight sleeve. Size is the minimum of the two gaps and the remaining daily cap.

## 0. Confirm the policy

Open the test policy the build loads. It must match this document.

Targets:

| Asset | Target weight | Target dollars |
| --- | --- | --- |
| BTC | 53% | 265,000 |
| SOL | 4% | 20,000 |
| USDC | 43% | 215,000 |

Other settings:

- `drift_band_percentage_points`: 5
- `max_trade_usd_per_day`: 10000
- `venues`: `near-intents`
- `withdrawal_addresses`: empty
- `staking_allowlist`: SOL only, instrument `JitoSOL`, plus a devnet fallback id `native-stake-devnet` if that is all the test chain supports
- `kill_switch`: false
- Manual sleeves and veAERO are not in this test policy. The reminder step is a calendar row, not a vote.

Recompute SHA-256 over the canonical JSON (UTF-8, keys sorted, no insignificant whitespace).

**Pass if** the hash equals the signature the investor account produced, and equals the hash stored by the guard.

## 1. Load fixture A and compute drift

Marks:

| Asset | Dollars | Weight | Target | Drift (points) | Outside band? |
| --- | --- | --- | --- | --- | --- |
| BTC | 290,000 | 58% | 53% | +5 | yes |
| SOL | 0 | 0% | 4% | -4 | no |
| USDC | 210,000 | 42% | 43% | -1 | no |

Sum of marks: 500,000.

Most overweight: BTC. Gap to target: 290,000 minus 265,000 = 25,000.

Most underweight: SOL. Gap to target: 20,000 minus 0 = 20,000.

Trade size: min(25,000, 20,000, 10,000) = 10,000.

Direction: sell BTC, buy SOL, venue NEAR Intents, destination the agent's own Solana address.

**Pass if** the build prints this size and this direction before it submits anything.

## 2. Run one rebalance

Run the agent once on fixture A.

Expected after the trade, still on a $500,000 NAV:

| Asset | Dollars | Weight | Drift (points) | Outside band? |
| --- | --- | --- | --- | --- |
| BTC | 280,000 | 56% | +3 | no |
| SOL | 10,000 | 2% | -2 | no |
| USDC | 210,000 | 42% | -1 | no |

The daily cap set the size. SOL did not reach its $20,000 target. The band is satisfied, so the agent stops. That is the rule, not a partial bug.

**Pass if** there is exactly one intent, notional $10,000, BTC to SOL, solver id present, and states `submitted` and `settled` are separate lines. **Pass on tabletop if** the same numbers are what a judge would accept from the log.

## 3. Run again the same day

Run the agent a second time with the post-trade marks from step 2, same calendar day.

**Pass if** it does not trade. Reason in the log: every sleeve is inside the band. Remaining daily cap is $0 if the first trade counted, but the band check is sufficient here.

## 4. Fixture B, cap still binding

New day, so the cap is reset. Marks:

| Asset | Dollars | Weight | Drift (points) | Outside band? |
| --- | --- | --- | --- | --- |
| BTC | 320,000 | 64% | +11 | yes |
| SOL | 0 | 0% | -4 | no |
| USDC | 180,000 | 36% | -7 | yes |

Sum: 500,000.

Most overweight: BTC. Gap: 320,000 minus 265,000 = 55,000.

Most underweight: USDC. Gap: 215,000 minus 180,000 = 35,000. SOL's gap is 20,000, which is smaller, so SOL is not the buy.

Trade size: min(55,000, 35,000, 10,000) = 10,000. Sell BTC, buy USDC.

After that single trade:

| Asset | Dollars | Weight | Drift (points) | Outside band? |
| --- | --- | --- | --- | --- |
| BTC | 310,000 | 62% | +9 | yes |
| SOL | 0 | 0% | -4 | no |
| USDC | 190,000 | 38% | -5 | yes |

BTC and USDC are still outside the band. The cap for the day is used.

**Pass if** the run submits exactly one $10,000 BTC-to-USDC intent and then stops with `daily_cap`. A second trade in the same run is a fail. A SOL buy in this fixture is a fail.

## 5. Stake the one allowlisted asset

On the SOL balance from fixture A ($10,000 in the fixture), record a stake action.

- Fixture or unit test: instrument `JitoSOL`, amount equal to the SOL sleeve, destination the allowlisted program, not a new investor address.
- Live leg: one devnet signature from `v1.signer` under the account's Solana derivation path. Amount is the tiny test amount, not $10,000. If the liquid-stake program is absent on devnet, instrument `native-stake-devnet` is acceptable. The log must say which one happened.

**Pass if** a judge can see one stake record, the destination is on the allowlist, and a live signature verifies for the derived address. **Fail if** the agent stakes ETH, USDC, or BTC. BTC is not staked in this policy.

## 6. Reminder, not a vote

Ask for the weekly job the full book uses for veAERO.

**Pass if** the log gains one reminder, with a date, and there is no vote transaction and no lock transaction. The test policy does not name an Aerodrome pool. With no pool rule, a vote would be the agent inventing a choice.

## 7. Refuse a new address

Request a withdrawal of 1 USDC to a fresh address that is not in `withdrawal_addresses`.

**Pass if** the guard rejects it, the action log contains the reject, and there is no call to `v1.signer` and no Intents settlement. Adding the address requires a new policy hash signed by the investor key. The agent key cannot do that update.

## 8. Kill switch

With the investor key, pause the guard. Then run fixture A again, which would otherwise trade.

**Pass if** the run logs `paused` and submits no intent and no signature. Call unpause with the agent key.

**Pass if** unpause is rejected. Unpause with the investor key, run a no-op check, and confirm the guard accepts calls again.

## 9. Quarterly report, tabletop

Read this sample as the report the stretch build would generate from the log. If the prototype already writes a report, the report replaces this sample and must be marked with the policy hash.

Sample, tied to fixture A, not to a live account:

- Period: demonstration run, 12 Nov 2026.
- Policy hash: the hash from step 0.
- One fill: $10,000 BTC to SOL through NEAR Intents.
- One stake record on the SOL sleeve.
- One veAERO reminder. No vote cast.
- One rejected withdrawal to a new address.
- No rule change requested.

**Pass if** a judge can see that the investor is asked to sign nothing in this sample. A signature line appears only when a target, a cap, an address, or an allowlist entry changes.

## 10. What this demo does not show

- It does not buy the full illustrative book, and it does not move XMR.
- It does not pay a fee to NEAR holders. That path is a proposal in the spec.
- It does not use $500,000 of funds. Judges should treat any live notional as the small amount printed in the log.

## Fail list

Any one of these fails the demo:

- More than one trade in a single fixture run.
- A trade above $10,000.
- A trade when every sleeve is inside the band.
- BTC staked, or any asset staked to a program that is not on the allowlist.
- A withdrawal to a new address, or a signature request on that path.
- A governance vote from the reminder step.
- A fixture result with no `source` label.
- The agent key clears the kill switch.
