# IBetU

> **Bet on anything. With anyone. Trustlessly.**

Decentralized peer-to-peer platform for wagers on arbitrary events — personal challenges, niche predictions, social bets, or public events. Define your terms, lock stakes in smart contracts, and resolve via a trusted friend or community consensus voting. Fees sustainably reward honest resolution participants.

**Current Status**: Planning & Architecture Phase • July 2026  
Multi-agent AI-assisted development (Claude + Grok). See issues and docs for active work. Ready for review and iteration.

## Concept in One Paragraph

You create a bet proposal specifying the event, clear settlement criteria, odds, your max stake, and whether it resolves via a nominated friend (low fee) or open community vote (higher fee pool that rewards voters). Others accept or counter with different terms. Funds escrow automatically. On resolution, winner claims the pot minus fees. The system aligns incentives so resolvers and voters are motivated to be accurate and fair, enabling low-trust social and speculative wagers at scale.

## Key Features (MVP Target)

- **Arbitrary Bet Creation**: Free-text event + structured settlement criteria (to reduce disputes), odds (decimal or fractional), stake limits, resolution mode (friend address or "Open Consensus"), resolution deadline.
- **P2P Matching with Counters**: Accept the exact terms or submit a counter-proposal (different odds/stake). Creator can accept counter or let it stand.
- **On-Chain Escrow**: USDC (or native) stakes locked in audited BetEscrow contract upon mutual acceptance. No custodial risk.
- **Hybrid Resolution**:
  - **Friend Mode** (~1-2% fee): Nominated resolver submits outcome. Optional bond + timelock (24-72h) + appeal path to consensus.
  - **Consensus Mode** (~5% fee): Time-boxed community vote. Voters participate (with skin-in-game stake), majority decides outcome. Fee pool distributed pro-rata to participants as reward (USDC primary).
- **Full Transparency**: All actions on-chain or IPFS-anchored. Public bet feed, history, resolution proofs.
- **Self-Custodial & Permissionless**: Connect wallet, no KYC for core flows (jurisdiction warnings apply).

## Why This Matters

Centralized betting is opaque, censored, and limited. Public prediction markets (Polymarket, etc.) excel at broad events but not personal/social wagers. IBetU fills the gap with decentralized infrastructure, economic incentives for resolution integrity, and support for subjective-yet-judgeable events through hybrid mechanisms.

Inspired by Polymarket, Augur, Kleros dispute resolution, and real-world friendly betting circles — but fully on-chain and scalable.

## Tech Stack (v0.1 Direction)

- **Contracts**: Solidity (Hardhat primary, Foundry for advanced testing). Deploy first to Base or Arbitrum (low fees, strong EVM tooling). USDC as primary token.
- **Frontend**: Next.js 14 (TS, App Router) + Tailwind + shadcn/ui + wagmi/viem + RainbowKit. IPFS for rich metadata.
- **Indexing**: The Graph or Ponder for fast queries on bets, positions, votes.
- **Future**: Solana Anchor exploration if EVM costs or throughput constrain; cross-chain via CCIP/LayerZero.

## Project Structure

```
IBetU/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── PROJECT_MEMORY.md      # Shared persistent context for agents & humans (READ FIRST)
│   ├── PROJECT_PLAN.md          # Phased roadmap, milestones, deliverables
│   ├── AGENT_WORKERS.md         # Role definitions, collaboration protocol
│   ├── SYSTEM_ARCHITECTURE.md   # Components, data flows, contract outlines, diagrams
├── contracts/                 # (Future) Solidity source, tests, scripts
├── frontend/                  # (Future) Next.js dApp
└── ...
```

## Getting Started for Contributors & AI Agents

1. **Read the memory doc first**: `docs/PROJECT_MEMORY.md` — this is the single source of truth for vision, decisions, terminology, and open questions.
2. Review `docs/AGENT_WORKERS.md` to understand your role and how to collaborate.
3. Check GitHub Issues (labels: `agent:*`, `component:*`, `type:spec|feature|research`).
4. Work in branches, open PRs linked to issues, update docs/memory on decisions.
5. All contract changes require tests + security notes.

## Legal & Compliance Framing

IBetU is experimental software for **entertainment, social coordination, and informational prediction purposes**. 

- Users are **solely responsible** for complying with gambling, securities, and contract laws in their jurisdiction.
- Strong disclaimers, ToS, and educational content will be included.
- Design is jurisdiction-aware (inspired by prior nft-proxy-gamble framing as entertainment/collectibles where appropriate).
- No promotion of illegal activity. Geo-fencing or warnings may be implemented where technically feasible.

**Not financial advice. Bet responsibly. This is alpha software.**

## Contributing

We use a structured multi-agent development model (detailed in AGENT_WORKERS.md). Humans + AIs (Claude as lead implementer/reviewer, Grok as PM/architect) collaborate via GitHub Issues, PRs, and shared memory docs.

- Rigorous review culture (especially security & economics).
- Update PROJECT_MEMORY.md with any material decision using `[Decision YYYY-MM-DD]` tags.
- Tests and docs are first-class.

PRs welcome once core is stable. Good first issues will be labeled.

## Roadmap Snapshot

See `docs/PROJECT_PLAN.md` for full phased plan.

- **Phase 0 (Now)**: Planning, specs, memory, agent framework, initial review by Claude Fable.
- **Phase 1**: Core escrow + friend-resolution contracts + basic tests + testnet deploy.
- **Phase 2**: Consensus voting + reward distribution + hybrid appeal flows.
- **Phase 3**: Frontend dApp (create/accept/counter/vote/claim flows) + indexer.
- **Phase 4**: Polish, security audit prep, public testnet, documentation, community onboarding.
- **Later**: Advanced matching, multi-outcome, oracle integrations (for objective events), mobile app, tokenomics expansion, multi-chain.

## Links

- Repository: https://github.com/dirac1974/IBetU
- Issues & Projects: (GitHub board for kanban)
- X / Twitter: (future @ibet u or project handle)

---

*Built with ❤️ for decentralized social coordination and truthful resolution mechanisms. Let's make betting on life events as seamless and fair as sending a message.*