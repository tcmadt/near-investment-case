# Investment case

Tobé Madu. 12 Nov 2026.

Hackathon argument. Estimates are labeled. This is not advice. The sample book is illustrative. Sources are in [SPEC.md](SPEC.md). Book figures are the author's and were not re-pulled from DefiLlama.

## Thesis

The portfolio the author would want is ongoing work: buys, rebalances, stakes, locks, and votes across chains. The author has been in crypto for more than ten years and still will not run that book by hand. The easier book is the one that gets held.

A NEAR agent turns the work into a few decisions a year. The investor signs allowed assets, target weights, a drift band, trade size, venues, and what may be staked or locked. The agent acts inside that policy, reports once a quarter, and asks for a new signature only when a rule changes. The author's line: only judgment is required from the investor, not action.

If agents run portfolios, the author's position is the rail those orders cross. The rail is NEAR Intents (one request, solvers compete, `intents.near` settles), chain signatures (one NEAR account calls `v1.signer` and receives a secp256k1 or ed25519 signature for Bitcoin, Solana, EVM chains, and other chains NEAR lists), and NEAR AI Cloud (TEE inference with attestation). The author describes that NEAR position as the toll road.

## Fees today, and the proposal

**Fact.** NEAR's Chain Signatures TEE post reports all-time Intents volume above $35 billion. Volume is not a holder payout. A writeup of the on-chain fee view recorded 1 pip, 0.0001%, in October 2026. Solver terms say the fee can change, that it goes to a protocol recipient, and that a capture share can accrue to the intents operator. Solver pages say the solver keeps the spread, net of that fee. Frontends add their own fees. This document shows no Intents distribution to NEAR holders. Read the live Verifier `fee` view before relying on the pip, because it can move.

**Inference.** At a pip-level protocol fee, solvers and frontends keep almost all of a fill. The rail can be busy while holders receive very little.

**Proposal, not live design.** The author wants HYPE-style alignment: solvers stake or hold NEAR to fill, bad fills can be slashed, and a protocol fee has an on-chain path to NEAR holders (a distribution to staked NEAR, or a buyback). Rate, minimum stake, and slash size are unset. Hyperliquid's fee flow and HYPE staking are the analogy only. Hyperliquid is not this solver set. The hackathon build does not ship the fee change.

## Competition, as the author argues

**Phantom.** The CEO has said there is no plan for a token. CASH is a US dollar stablecoin. Holding it does not pay the holder Phantom's fees. There is no Phantom asset for "agent flow routes here."

**Coinbase (COIN).** The equity bundles the exchange, custody, Base, a role in USDC, and other lines. It is a wide expression of a thesis about one rail.

**Other agent stacks.** The author's argument is that they lack intent settlement and chain signatures on one account. Other bridges and routers already move assets across chains. That is a real competing path.

## Sample book, and what would make this wrong

The spec's illustration is $500,000 at the author's weights. Revenue-to-cap ratios use the author's 12 Nov 2026 reading of last-30-day DefiLlama revenue, annualized, over market cap. They measure protocol revenue against market cap. They are not the cash yield on this book. BTC is 53%, native, unstaked. The author's HYPE pair, $826 million of annualized revenue on a $17 billion cap, is about 4.9%.

**Estimate (author).** Staking, locks, and governance rewards: about 1.5% to 2% of NAV per year before price moves and before slashing, or about $7,500 to $10,000 on $500,000. Named rates only, weighted: HYPE stake 2% to 2.5% on 10% (midpoint 2.25%), stETH about 3% on 4%, JitoSOL about 7% on 4%. Sum about 0.63 points of NAV, about $3,100 a year. The rest of the 1.5% to 2% is yield on sleeves with no rate in the notes. Those rates are left blank. AAVE safety-module stake can be slashed.

The thesis fails if volume stays on Intents and holders still get no meaningful fee. It fails if wallets or other agent stacks take the flow. Phantom can win the user and still leave nothing public to own. It fails if one bad signature, bad allowlist entry, or stolen key makes the agent unsafe to leave on. It fails if DeFi activity drops and the sample revenue ratios and stake yields fall with it. A product sold to other people can be treated as advice, brokerage, or custody. This spec is a self-directed policy on the investor's own account, and it is not a legal opinion.

Judgment stays with the investor. NEAR is the author's bet on where the allowed actions settle. That bet needs a holder fee path that is not live, and the guardrails in the spec.
