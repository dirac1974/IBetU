# IBetU Project Plan

**Status**: Phase 0 in progress • July 2026  
**Goal**: Deliver a secure, usable MVP enabling friend-resolved and basic consensus-resolved arbitrary bets end-to-end on testnet within 8-12 weeks of active development (AI-accelerated).

## Guiding Principles for Planning
- Security & economic soundness > speed (especially contracts and incentives).
- Iterate in thin vertical slices: get a full friend-resolved bet flow working early, then add consensus.
- Parallelize where possible (contracts + frontend mocks, research + impl).
- Document everything; update memory on decisions.
- Leverage AI agents heavily for code gen, review, test writing, docs.

## Phase 0: Foundation, Specs & Review (1-2 weeks, Current)

**Objective**: Solidify architecture, incentives, scope, and collaboration model. Get external (Claude Fable) + user review. Produce actionable specs.

**Deliverables**:
- [x] Repo + README + core docs (PROJECT_MEMORY, this PLAN, AGENT_WORKERS, SYSTEM_ARCHITECTURE)
- [ ] Detailed contract interface specs (BetEscrow.sol, structs, functions, events, state machine) in docs/specs/
- [ ] Simple economic/financial model (Python notebook or Google Sheet) for fee flows, break-even voter participation, example bet scenarios
- [ ] Initial legal/compliance framing draft (ToS skeleton, disclaimers, jurisdiction notes)
- [ ] Low-fidelity UX flows / wireframes (textual or Figma link if created)
- [ ] Refined open questions list with proposed v1 solutions
- [ ] Claude Fable full review of all planning docs + initial opinions on incentives, chain choice, scope
- [ ] User (Vernier) feedback and prioritization decisions
- [ ] Updated PROJECT_MEMORY.md with Phase 0 decisions

**Success Criteria**: All stakeholders aligned on v1 scope, incentive model, tech choices, and development process. No major blockers identified.

**Dependencies**: None (pure planning).

**Risks & Mitigations**: Over-scoping → Strict MVP focus on friend mode + basic consensus. Ambiguous incentives → Dedicated research sub-task + modeling.

## Phase 1: Core Smart Contracts — Escrow + Friend Resolution MVP (3-4 weeks)

**Objective**: Deployable, tested contracts for creating, accepting, resolving (friend), and claiming on testnet. Full vertical slice for friend bets.

**Deliverables**:
- `contracts/BetEscrow.sol` (or BetFactory + Escrow) with:
  - Bet struct + state machine (Open → Accepted → Resolved → Claimed or Disputed)
  - createBet(), acceptBet(), submitCounter(), cancelBet(), resolveByFriend(), claimWinnings()
  - Events for indexing
  - IPFS CID + hash storage
  - USDC (IERC20) integration with SafeERC20
  - Timelock & basic bond logic for resolver (optional in v1)
- Comprehensive test suite (Hardhat): unit tests for happy path + edge cases (reverts, reentrancy attempts, timing), >95% coverage
- Fork tests simulating real Base/Arbitrum conditions
- Deployment scripts + verification (Hardhat)
- Testnet deployment (Base Sepolia or Arbitrum Sepolia) with 3-5 example bets created/resolved via scripts
- Basic NatSpec + developer docs in contracts/
- Update SYSTEM_ARCHITECTURE.md and MEMORY with any refinements
- Security notes / threat model for friend resolution

**Success Criteria**: End-to-end friend-resolved bet works on testnet via scripts (create → accept → resolve → claim). No critical bugs. Gas costs reasonable for L2.

**Dependencies**: Phase 0 specs approved.
**Parallel Work**: Frontend team can start UI components + wagmi hooks using contract ABIs/interfaces (mocked or early deploys).

**Risks**: Reentrancy or access control bugs → Mandatory security patterns + external review. Timing/ deadline logic errors → Thorough property testing.

## Phase 2: Consensus Voting, Rewards & Hybrid Flows (3-4 weeks)

**Objective**: Add open consensus resolution path, voter participation mechanics, reward distribution, and appeal/dispute from friend resolutions.

**Deliverables**:
- `contracts/ConsensusVoting.sol` (or module in main escrow):
  - vote(), tally(), claimVoterReward()
  - Eligibility/stake collateral logic
  - Time windows, quorum or majority rules (simple or stake-weighted)
  - Integration with BetEscrow (resolve via vote or appeal trigger)
- Reward distribution math implemented & tested (pro-rata, perhaps with bonus for majority alignment)
- Dispute/appeal flow: triggerDispute() from BetEscrow, funding voter pool from dispute bond
- Expanded tests (voting scenarios, sybil simulation via multiple accounts, edge timing)
- Gas & participation modeling (how many voters realistic?)
- Update fee math and economic model
- Docs: RESOLUTION_DESIGN.md deep dive (incentives, failure modes, game theory notes)
- Testnet demo: Open consensus bet + multiple "voters" (agent or multi-wallet) resolving it + reward claims

**Success Criteria**: Both resolution modes work end-to-end. Voters receive USDC rewards. Appeal path functional. Clear documentation of incentive model and known limitations.

**Dependencies**: Phase 1 contracts stable. Economic model from Phase 0.

**Risks**: Poor incentive design leading to bad equilibria (apathy, collusion) → Heavy research + simulation in parallel. Gas costs for popular votes → Optimize or note as L2 limitation.

## Phase 3: Frontend dApp & User Experience (Parallel to Phase 2, 4-5 weeks total)

**Objective**: Production-ready web interface making complex flows feel simple, safe, and delightful. Full CRUD for bets + voting.

**Deliverables**:
- Next.js 14 app (TypeScript, Tailwind, shadcn/ui)
- Wallet connection (RainbowKit or ConnectKit) + wagmi/viem hooks for all contract interactions
- Key pages/routes:
  - `/` or `/discover`: Feed of open bets (search, filter by mode/expiry/stake, sort)
  - `/create`: Multi-step form (event desc + criteria builder, odds, stake, resolver choice, IPFS upload preview)
  - `/bet/[id]`: Detail view with accept/counter actions, criteria display, current status, vote UI if consensus
  - `/dashboard`: My bets (active, history), claimable rewards, resolution reputation
- IPFS upload + retrieval integration (with hash verification)
- Transaction feedback (toasts, pending states, error messages with reasons)
- Responsive design (desktop-optimized for complex views, mobile usable)
- Basic The Graph or Ponder indexer integration (or wagmi polling for v0.5)
- Loading/empty/error states, accessibility basics
- Update docs/UX_FLOWS.md or similar
- E2E smoke tests (Playwright or manual + script)

**Success Criteria**: Non-technical user can create a friend bet, have it accepted by another wallet, resolved, and winnings claimed entirely via UI on testnet. Voting flow intuitive.

**Dependencies**: Phase 1 contracts (for ABI/types). Can run parallel with mocks.

**Risks**: Complex state management or transaction UX friction → Early user testing (even internal) + clear confirmations. IPFS unreliability → Robust gateway + fallback + on-chain verification.

## Phase 4: Polish, Audit Prep, Testnet Public Beta (2-3 weeks)

**Objective**: Harden for public testnet launch, prepare for security audit, create onboarding materials.

**Deliverables**:
- Full test coverage + invariant/fuzz testing
- Slither + manual security review; fix findings
- Comprehensive documentation (user guide, FAQ, criteria writing best practices, "How resolution works" explainer)
- ToS, disclaimers, risk disclosures (legal input)
- GitHub Actions CI (lint, test, build, verify on PR)
- Public testnet deployment + faucet instructions + demo video/script
- Onboarding content: example bets ("Will it rain tomorrow?", personal challenge examples), video walkthroughs
- Community channels setup (if desired: Discord/Telegram/X)
- Bug bounty program skeleton (optional early)
- Update all docs, changelog, README

**Success Criteria**: Testnet is stable, documented, and usable by external testers. No critical/high security issues open. Ready for mainnet consideration or further audit.

**Dependencies**: Phases 1-3 complete.

**Risks**: Audit delays or findings → Budget time + prioritize fixes. Legal review scope creep → Early engagement with framing.

## Phase 5+: Future Enhancements (Post-MVP)

- Advanced matching / on-chain order book or simple AMM for popular bets
- Multi-outcome bets & share tokens (Polymarket-like)
- Optimistic oracle / Chainlink integration for objective events (sports, weather, prices)
- Mobile app (React Native leveraging prior experience) or PWA
- Platform token ($IBET) for governance, fee discounts, voting boosts
- Multi-chain expansion + cross-chain bets
- Resolver reputation system / on-chain history
- Insurance / staking pools for resolvers and disputers
- Analytics dashboard, leaderboards, social features
- Full DAO transition for treasury/protocol params

## Timeline Estimate (AI-Accelerated)

Assuming dedicated agent effort + human oversight:
- Phase 0: 1-2 weeks
- Phase 1: 3-4 weeks
- Phase 2: 3-4 weeks (parallel with frontend start)
- Phase 3: 4-5 weeks
- Phase 4: 2-3 weeks
**Total to public testnet beta**: ~12-16 weeks calendar (faster with heavy parallelism and AI code gen).

**Milestone Tracking**: Use GitHub Projects / Milestones linked to issues. PM updates status weekly or on major completions.

## Success Metrics (Overall MVP)

- Functional end-to-end flows for both resolution modes on testnet
- >95% test coverage on core contracts
- Gas costs acceptable for target L2 (< $0.50-2 per typical tx)
- Clear, documented incentive model with acknowledged limitations
- Positive internal + early external tester feedback on UX and trust
- All planning docs maintained and accurate

This plan is living — update as we learn. Prioritize ruthlessly for MVP while keeping extensibility.