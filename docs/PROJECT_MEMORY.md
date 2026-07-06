# IBetU Project Memory

**Purpose**: This is the persistent, shared context document for all AI agents (Claude, Grok, future specialists) and human contributors. **Read this first** before starting any task, coding, or review. It captures vision, terminology, current decisions, open questions, and conventions. Update it whenever material progress or decisions occur. Tag updates with `[Decision YYYY-MM-DD]` or `[Update YYYY-MM-DD]`.

## Project Vision

Build the most open, incentive-aligned, decentralized platform for people to bet on *arbitrary* events with friends or strangers — using smart contracts for trustless escrow and novel resolution mechanisms that handle both objective and subjective outcomes fairly.

**Core Principles**:
- Decentralization & self-custody first (minimize trusted parties for funds).
- Incentive compatibility: Resolution participants (friends or voters) are economically motivated toward accuracy and fairness.
- Clarity & reduced ambiguity: Bets require explicit upfront settlement criteria.
- User choice & sovereignty: Counter-proposals, resolver type selection, self-custody.
- Sustainable economics: Fees cover resolution rewards + platform sustainability (treasury for audits, ops, future features, insurance).
- Responsible design: Jurisdiction-aware, strong disclaimers, user education on risks and legality.

## Current Status (2026-07-05)

- Repository created and initialized with planning docs.
- Multi-agent framework and detailed memory/plan/architecture docs established.
- **No contracts or frontend code yet** — pure planning/spec phase.
- Awaiting Claude Fable review + user feedback to iterate on architecture, incentives, scope, and priorities.
- Next immediate steps: Refine open questions, produce detailed contract interfaces/specs, simple economic model, legal framing draft.

## Terminology

- **Bet Proposal / Bet**: The core entity. Includes event description (IPFS), settlement criteria (human + ideally verifiable), odds/payout multiplier, creator stake offer, max acceptable counter-stake, resolver mode (Friend or Consensus), nominated resolver address (if Friend), deadlines.
- **Taker / Counter-Party**: The user who accepts or counters the proposal, taking the opposite outcome.
- **Resolver (Friend Mode)**: Trusted EOA or contract nominated by creator (mutually acceptable). Submits the outcome via `resolve()` transaction. May post bond.
- **Voter / Consensus Participant**: Any eligible user who stakes collateral and casts a vote (Yes/No/Draw or categorical) within the voting window for Open Consensus bets.
- **Fee**: Percentage (1-5%) taken from the total pot or stakes at resolution, funding resolver/voter rewards and platform treasury.
- **Resolution Reward Pool**: Accumulated fees from Consensus bets, distributed to voters (pro-rata by stake weight or participation) and/or platform.
- **Counter-Proposal**: Alternative terms (odds, stakes, perhaps different resolver) offered in response to an original proposal. Can be accepted to create the bet under new terms.
- **Settlement Criteria**: The predefined rules that determine the outcome. Must be as unambiguous as possible (e.g. "If official NOAA reports Las Vegas high temp >= 110°F on 2026-07-15" or "If [named person] achieves [specific milestone] by [date] as confirmed by [source]"). Subjective criteria rely on resolver judgment.
- **Appeal / Dispute**: Process where a party challenges a Friend resolution by triggering a community vote (paying a dispute bond that funds voters).

## Key Decisions & Assumptions (Chronological)

**[2026-07-05 - Initial Setup]**
- Primary chain for v0.1: EVM L2 (Base or Arbitrum recommended for low fees + tooling). Defer Solana decision until after basic flows validated or if gas/throughput analysis shows need.
- Settlement currency: USDC (reliable, stable, widely supported). Native token optional for gas only.
- v1 Scope Focus: Friend resolution MVP first (simpler, lower fee, social use-case strong), then layer Consensus voting + hybrid appeal. Binary outcomes + Draw/Refund option.
- No new token launch in v1. Use USDC for fees, stakes, and voter rewards. Platform treasury cut from fees (e.g. 20-40% of collected fees) for sustainability.
- Metadata: Long descriptions and rich criteria on IPFS (CID stored + keccak hash on-chain for integrity). Frontend resolves via public gateways or dedicated pinning.
- Matching v1: Public proposals list + direct accept or counter. No complex on-chain order book or AMM yet (gas + complexity). Creator can cancel unaccepted proposals.
- Fee Structure (proposed, to model): 
  - Friend Mode: ~1.5% total fee on pot → ~0.5% to resolver (incentive), ~1% to platform treasury.
  - Consensus Mode: ~5% total → ~1.5-2% platform, ~3-3.5% to voter reward pool (distributed pro-rata to voters who participated).
- Legal Framing: "Decentralized protocol for social event wagering and prediction markets. For entertainment and informational purposes only. Users must comply with all applicable laws in their jurisdiction. Strong disclaimers and educational content required."
- Development Model: Multi-agent AI collaboration (detailed in AGENT_WORKERS.md) with Claude focused on implementation depth, Grok on architecture/PM/review, structured via GitHub Issues/PRs + this memory.

## Open Questions & Risks (Prioritize Resolution in Phase 0/1)

1. **Voter Truthfulness & Incentives (Biggest Challenge)**: Without easy ground truth for arbitrary events, how do we reward "correct" votes? 
   - Option A (Simple v1): Flat or stake-weighted participation reward from fee pool. Easy to implement, but may attract lazy/spam votes.
   - Option B: Reward only voters whose vote aligned with final majority outcome (encourages coordination but risks herding/bandwagon, not independent judgment).
   - Option C: Bonded voting + slashing for being in minority if later disputed/appealed (more skin-in-game, Kleros-like).
   - Option D: Hybrid with optimistic elements or future UMA integration for objective sub-events.
   **v1 Recommendation**: Start with participation rewards + require meaningful stake collateral per vote (returned + reward share). Research Schelling mechanisms further. Document game-theoretic risks.

2. **Sybil Resistance & Collusion for Open Votes**: Permissionless voting is vulnerable to wallet farming. Mitigations:
   - Stake collateral per vote (e.g. 5-20 USDC, returned + bonus).
   - Future: Token-gated (but bootstrap chicken-egg), Gitcoin Passport, or on-chain reputation from past fair resolutions.
   - For Friend mode: Social graph or on-chain resolution history as soft signal (not enforced initially).

3. **Dispute & Appeal Mechanism**: Should a Friend resolution be final immediately, or have a short appeal window where losing party can force community vote by posting dispute fee/bond? Strongly recommended for hybrid robustness. Details: bond amount, who pays voters if appeal succeeds, timelock interaction.

4. **Exact Fee Splits & Treasury %**: Model cash flows. What % to resolver vs voters vs platform? Should platform take cut from Friend mode too? Include insurance/staking fund for future resolver bonds or bad debt?

5. **Outcome Types**: Binary (Win/Lose) + Draw/Refund in v1. Support multi-outcome (shares like Polymarket) later? Or categorical votes.

6. **Chain Throughput & Costs**: Simulate gas for a popular bet with 50-200 voters on Base. Is it acceptable or do we need L3 / Solana sooner?

7. **Frontend Priority (Web vs Mobile)**: Betting UIs benefit from screen real estate for criteria review and voting. Web (Next.js) first for complexity, then PWA or React Native wrapper / separate app (leveraging prior nft-proxy-gamble RN experience).

8. **Future Token ($IBET?)**: Governance, voting power boost, or reward multiplier? Or stay tokenless (USDC only) longer? Regulatory implications strong consideration.

9. **Oracle Integration Path**: For objective events (sports scores, weather, prices) integrate Chainlink/UMA/RedStone later. Subjective/personal events stay human-resolved.

## Project Conventions & Standards

- **Contract Naming & IDs**: `BetEscrow` main contract. Bet IDs: `uint256` (e.g. keccak256(abi.encodePacked(creator, salt, block.timestamp)) or simple counter).
- **Events**: Comprehensive (BetCreated, BetAccepted, CounterProposed, BetResolved, VoteCast, RewardsClaimed, DisputeOpened, etc.). Indexed fields for The Graph.
- **Security Baseline**: OpenZeppelin contracts (SafeERC20, ReentrancyGuard, Pausable, AccessControl where minimal admin needed). Checks-effects-interactions pattern. Full NatSpec. Target 100% branch coverage on core logic.
- **Testing**: Hardhat + Chai + Ethers. Foundry for fuzzing/invariants where helpful. Fork tests against Base mainnet for realistic scenarios.
- **Documentation**: Every material decision or spec change updates this file + relevant docs/*. Use clear [Decision] tags. Specs live in docs/specs/ (to be created).
- **IPFS**: Use reliable pinning (Pinata, web3.storage, or self). Frontend must handle gateway fallbacks and content verification via on-chain hash.
- **Error Handling & UX**: Contracts revert with clear custom errors. Frontend surfaces human-readable reasons and next steps.
- **Agent Collaboration**: See AGENT_WORKERS.md. All work via Issues + PRs. Update memory on decisions. PM (Grok or human) coordinates.

## References & Prior Art

- Polymarket (order book + resolution for public events)
- Augur (decentralized prediction markets, reputation tokens)
- Kleros (crowdsourced dispute resolution, token incentives)
- UMA (optimistic oracle, dispute resolution)
- User's prior work: nft-proxy-gamble (ERC-1155 vouchers, provably-fair on-chain game, jurisdiction framing), BlockAssist (Solana P2P escrow), DragonForge (multi-agent casino math)
- Traditional: Escrow betting circles, private prediction markets, bookie models

**This memory is the living source of truth. Keep it accurate, concise, and actionable.**