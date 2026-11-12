# NEAR portfolio agent

Tobias Marlow. 12 Nov 2026. Hackathon entries close 14 Nov 2026, 23:59 UTC.

The portfolio an experienced investor would want is often too much work to run: buying, rebalancing, staking, locking, and voting across chains. The easier book is the one that actually gets held. This repository specifies a NEAR agent that takes a signed policy a few times a year and does the actions in between, through NEAR Intents and chain signatures, with spend caps, an address allowlist, an action log, and a kill switch. The investment case is that if agents run portfolios, the rails they cross collect the flow, and that NEAR is Tobias Marlow's expression of that bet. A fee path from Intents to NEAR holders is stated as a proposal. It is not how fees work today.

- [Investment case](docs/INVESTMENT_CASE.md)
- [Hackathon spec](docs/SPEC.md)
- [Demo script](docs/DEMO.md)
