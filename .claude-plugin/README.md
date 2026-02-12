# Claude Solidity Skills Plugin

A Claude Code plugin providing production-grade skills for Solidity development. Each skill encodes battle-tested methodology from the best public resources in the ecosystem.

## Installation

### One-Click Plugin Install (Recommended)

```bash
# Add marketplace
/plugin marketplace add max-taylor/Claude-Solidity-Skills

# Install plugin
/plugin install Claude-Solidity-Skills@solidity-skills
```

Or via CLI:

```bash
claude plugin marketplace add max-taylor/Claude-Solidity-Skills
claude plugin install Claude-Solidity-Skills@solidity-skills
```

### Local (for development)

```bash
claude --plugin-dir ./solidity-skills
```

## Skills

### `/solidity-skills:test-hardhat` — Hardhat Test Generation

Generates comprehensive Hardhat v3 test suites with structured coverage across unit, integration, and end-to-end testing layers.

**What it does:**

- Reads and analyzes the target contract's full interface (functions, modifiers, events, errors, state)
- Produces a test plan covering deployment, per-function happy/sad paths, access control, boundary cases, and multi-step integration flows
- Writes TypeScript tests using `loadFixture`, Chai matchers, and `hardhat-network-helpers`
- Outputs a coverage summary mapping tests to every require, event, and modifier

**Invoke:**

```
/solidity-skills:test-hardhat MyContract.sol
```

**Resources used to build this skill:**

- [Hardhat v3 Testing Guide](https://hardhat.org/docs/guides/testing) — Hardhat's official dual Solidity/TypeScript testing approach, fixtures, and network helpers
- [Moloch DAO Test README](https://github.com/MolochVentures/moloch/blob/master/test/README.md) — Verification helper pattern, DRY test philosophy, per-require coverage discipline, and state machine testing methodology
- [Smart Contract Security Field Guide — Testing](https://scsfg.io/developers/testing/) — Test planning hierarchy (unit → integration → e2e → fuzz), TDD workflow, invariant testing philosophy
- [Ethereum.org — Smart Contract Testing](https://ethereum.org/developers/docs/smart-contracts/testing/) — Canonical testing taxonomy (unit, integration, property-based, static/dynamic analysis), tool landscape overview

---

### `/solidity-skills:test-foundry` — Foundry/Forge Test Generation

Generates comprehensive Forge test suites with first-class support for fuzz testing, invariant testing, and fork testing.

**What it does:**

- Reads and analyzes the target contract's full interface
- Produces a test plan covering unit tests, fuzz tests, invariant tests with handler contracts, and fork tests
- Writes Solidity tests using forge-std's `Test` base, cheatcodes (`vm.prank`, `vm.expectRevert`, `vm.expectEmit`, `vm.warp`, `deal`, etc.), and `bound()` for input constraining
- Generates handler contracts for invariant testing with ghost variables and multi-actor simulation
- Outputs a coverage summary including fuzz and invariant coverage

**Invoke:**

```
/solidity-skills:test-foundry MyContract.sol
```

**Resources used to build this skill:**

- [Foundry Book — Writing Tests](https://book.getfoundry.sh/forge/writing-tests) — Test structure, `setUp()`, naming conventions (`test_`, `testFuzz_`, `invariant_`), assertion library, verbosity flags
- [Foundry Book — Cheatcodes Overview](https://book.getfoundry.sh/forge/cheatcodes) — Full `vm.*` cheatcode reference (prank, expectRevert, expectEmit, warp, roll, deal, store, mockCall, etc.)
- [Foundry Book — Invariant Testing Guide](https://book.getfoundry.sh/guides/invariant-testing) — Handler pattern, ghost variables, `targetContract`, `targetSelector`, `bound()` vs `vm.assume()`, multi-actor simulation, configuration
- [Foundry Book — Fork Testing Guide](https://book.getfoundry.sh/guides/fork-testing) — `vm.createFork`, `vm.selectFork`, block pinning, `deal()` for ERC20s, RPC caching
- [Moloch DAO Test README](https://github.com/MolochVentures/moloch/blob/master/test/README.md) — Verification helper pattern, per-require coverage discipline, state machine testing
- [Smart Contract Security Field Guide — Testing](https://scsfg.io/developers/testing/) — Test planning hierarchy, fuzz testing philosophy, invariant property identification
- [Ethereum.org — Smart Contract Testing](https://ethereum.org/developers/docs/smart-contracts/testing/) — Testing taxonomy and property-based testing methodology

---

### `/solidity-skills:gas-optimize` — Gas Optimization Analysis

Analyzes a Solidity contract for gas optimization opportunities, ranked by impact with before/after code and estimated gas savings.

**What it does:**

- Analyzes storage layout, access patterns, and function hot paths
- Identifies savings across 7 categories: storage layout, data structures, function-level, arithmetic, assembly, compiler settings, and L2-specific considerations
- Provides before/after code with specific gas numbers for every recommendation
- Outputs a prioritized report (high/medium/low impact)

**Invoke:**

```
/solidity-skills:gas-optimize MyContract.sol
```

**Resources used to build this skill:**

- [Cyfrin — 11 Solidity Gas Optimization Tips](https://www.cyfrin.io/blog/solidity-gas-optimization-tips) — 11 techniques with Foundry benchmarks and real gas savings numbers (90% on events vs storage, 89% on mappings vs arrays, etc.)
- [Alchemy — Solidity Gas Optimization](https://www.alchemy.com/overviews/solidity-gas-optimization) — 12 techniques with code examples, encoding patterns, unchecked arithmetic, calldata vs memory, and gas cost reference table
- [Hacken — Solidity Gas Optimization](https://hacken.io/discover/solidity-gas-optimization/) — Storage packing, gas refund mechanisms (15,000 refund on zero), compiler optimizer settings, `bytes32` vs `string`
- [Cyfrin — L2 Gas Efficiency Tips](https://www.cyfrin.io/blog/solidity-gas-efficiency-tips-tackle-rising-fees-base-other-l2) — L2-specific considerations (calldata dominates costs on rollups, storage relatively cheaper, batch operation value)

---

### `/solidity-skills:audit` — Security Audit

Performs a systematic, checklist-driven security audit with SWC vulnerability classification and weird ERC20 edge case analysis.

**What it does:**

- Works through 115+ checklist items across variables, structs, functions, modifiers, code patterns, external calls, events, and contract-level concerns
- Scans for all 37 SWC (Smart Contract Weakness Classification) vulnerability types
- Checks token interactions against 20+ known weird ERC20 behaviors (fee-on-transfer, rebasing, missing return values, blocklists, etc.)
- Includes DeFi-specific checks (oracle manipulation, flash loans, first depositor attacks, rounding direction)
- Outputs a structured audit report with findings classified by severity (Critical/High/Medium/Low)

**Invoke:**

```
/solidity-skills:audit MyContract.sol
```

**Resources used to build this skill:**

- [Smart Contract Audit Checklist](https://github.com/tamjid0x01/SmartContracts-audit-checklist) — 115+ actionable checklist items covering variables (10), structs (3), functions (19), modifiers (3), code patterns (51), external calls (8), static calls (4), events (5), contract-level (12)
- [Awesome Audits Checklists](https://github.com/TradMod/awesome-audits-checklists) — Meta-list linking to Cyfrin Solodit, ERC-specific checklists (ERC20, ERC721, ERC4626, ERC4337), DeFi protocol checklists (AMM, CDP, LSD), oracle security, bridge security
- [SlowMist Auditor Learning Roadmap](https://github.com/slowmist/SlowMist-Learning-Roadmap-for-Becoming-a-Smart-Contract-Auditor) — Vulnerability taxonomy referencing DASP Top 10, SWC registry, and EVM-specific exploit patterns
- [Weird ERC20 Tokens](https://github.com/d-xo/weird-erc20) — Complete catalog of non-standard ERC20 behaviors: missing return values (USDT), fee-on-transfer (STA, PAXG), rebasing (AMPL), flash minting (DAI), blocklists (USDC), pausable (BNB), approval race conditions, low/high decimals, and more
- [SWC Registry](https://swcregistry.io/) — Standardized vulnerability classification (SWC-100 through SWC-136) mapping to CWE IDs, used as the industry-standard reference in audit reports
- [Smart Contract Security Field Guide — Testing](https://scsfg.io/developers/testing/) — Security-focused testing methodology overlapping with audit concerns

## Plugin Structure

```
solidity-skills/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── test-hardhat/
│   │   └── SKILL.md            # Hardhat test generation
│   ├── test-foundry/
│   │   └── SKILL.md            # Foundry test generation
│   ├── gas-optimize/
│   │   └── SKILL.md            # Gas optimization analysis
│   └── audit/
│       └── SKILL.md            # Security audit
└── README.md
```

## Contributing

To add a new skill, create a directory under `skills/` with a `SKILL.md` file. See existing skills for the frontmatter format and methodology structure.
