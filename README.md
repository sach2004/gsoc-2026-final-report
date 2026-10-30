# GSoC 2026 Final Report: Gluon-EVM

| | |
|---|---|
| **Contributor** | Sachin S Thankachan ([@sach2004](https://github.com/sach2004)) |
| **Organization** | AOSSIE, Stability Nexus |
| **Mentors** | Bruno ([@Zahnentferner](https://github.com/Zahnentferner)), Sarthak Dengre ([@DengreSarthak](https://github.com/DengreSarthak)), Aditya Bhattad ([@Aditya](https://github.com/adityabhattad2021)), José Miguel Avellana |
| **Project page** | [GSoC 2026: Gluon Multi-Oracle Reactor Architecture for EVM](https://summerofcode.withgoogle.com/programs/2026/projects/l3AkqB4t) |
| **Project repos** | [Gluon-EVM](https://github.com/StabilityNexus/Gluon-EVM), [Gluon-EVM-WebUI](https://github.com/StabilityNexus/Gluon-EVM-WebUI), [OrbOracle-Solidity](https://github.com/StabilityNexus/OrbOracle-Solidity) |
| **Work record** | 1 May to 30 October 2026. This is the contribution period I used here, not the official GSoC coding dates. |
| **Report version** | Final report, updated 30 October 2026. |
| **Maintainer details** | [Contracts MAINTAINERS.md](https://github.com/StabilityNexus/Gluon-EVM/blob/main/MAINTAINERS.md) and [WebUI MAINTAINERS.md](https://github.com/StabilityNexus/Gluon-EVM-WebUI/blob/main/MAINTAINERS.md) |

## Introduction

Gluon is a DeFi protocol where a user deposits a reserve token and gets two tokens called Neutron and Proton. Neutron is the stable side and Proton is the part that takes the remaining price changes. The reactor handles fission, fusion, transmutation and the price calculations.

At the start, the contract was connected mainly to Pyth. My project was to make the oracle part more general, so the reactor can work with other price oracles like Chainlink and OrbOracle. I also worked on the WebUI so a user can choose an oracle, create a reactor and use the contracts from the browser.

## Goals of the Project

- **Done:** Use a common `IOracle` interface in the reactor and factory instead of keeping the protocol tied to one oracle.
- **Done:** Add Chainlink support with an adapter and tests.
- **Done with a change in plan:** Use OrbOracle directly through `IOracle`. We first looked at a separate Orb adapter, but later moved to the direct interface after discussion with the maintainers.
- **Changed from the proposal:** A separate Pyth adapter was not added to the final generic oracle path. The old Pyth-specific flow was removed when the interface direction changed.
- **Done:** Make the WebUI support the oracle choices, reactor creation and checks before deployment.
- **Done:** Test Orb and Chainlink reactors on Sepolia and record the transaction and deployment details.
- **Done:** Add tests, fix problems found during development and improve the documentation.
- **Done:** Verify Factory deployments on Ethereum, Polygon, Base, BSC, Ethereum Classic and Sepolia, and connect the verified addresses to the WebUI. These deployments are Factories, not proof of fully tested live Reactor markets on every mainnet.
- **Next step:** Finish the remaining security and live mainnet flow checks before treating the project as ready for real funds.

## Summary of achievements

During GSoC I worked on both the contracts and the frontend. The main thing I wanted to finish was the generic oracle support. Now Gluon can use Chainlink through the adapter and Orb directly through `IOracle`. I also updated the frontend to create and use these reactors.

By 30 October, I also verified Factory deployments on five mainnets and Sepolia and tested a fresh Reactor on Sepolia. Creation, fission, fusion, and both beta transmutations succeeded on-chain, with the token balances, fees and reserve accounting checked against the receipts.

I am most happy with getting the full flow working on Sepolia. It helped me find smaller problems with the amounts, fees, wallet changes and quotes which were not so clear just by reading the code. I fixed those and added tests where it was needed. The proposal was mainly about the oracle work, but later I also worked a lot on pricing, reserve calculations and the UI because those parts needed to match the latest contracts.

The most difficult part for me was keeping everything matching when the contract functions were changing. I had to check the raw amounts, decimals and actual transactions again and again. I discussed the design with the mentors, fixed the code and tested it again.

## Work Completed

### Features Added

- **Oracle integration:** Replaced the old provider-specific price reads with `IOracle`, added Chainlink support, updated the interface names and value interval functions, and tested direct Orb support. Related PRs include [Contracts #22](https://github.com/StabilityNexus/Gluon-EVM/pull/22), [#27](https://github.com/StabilityNexus/Gluon-EVM/pull/27), [#29](https://github.com/StabilityNexus/Gluon-EVM/pull/29) and [#43](https://github.com/StabilityNexus/Gluon-EVM/pull/43).
- **Reactor accounting:** Worked on reserve unit conversion, rounding, the alpha peg adjustment, reserve ratio checks, beta parameters, directional volume, developer fees and minimum output checks. Some of the main work is in [Contracts #36](https://github.com/StabilityNexus/Gluon-EVM/pull/36), [#54](https://github.com/StabilityNexus/Gluon-EVM/pull/54), [#58](https://github.com/StabilityNexus/Gluon-EVM/pull/58), [#59](https://github.com/StabilityNexus/Gluon-EVM/pull/59), [#62](https://github.com/StabilityNexus/Gluon-EVM/pull/62) and [#67](https://github.com/StabilityNexus/Gluon-EVM/pull/67).
- **WebUI oracle and deployment flow:** Added Chainlink and Orb options, oracle compatibility checks, network configuration, reactor explorer changes and Sepolia deployment flow. Related work is in [WebUI #28](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/28), [#29](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/29), [#33](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/33), [#40](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/40) and [#42](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/42).
- **WebUI interactions:** Updated native asset wrapping and unwrapping, split a large interaction page into hooks and components, improved the dashboard and creation form, and matched the on-chain rounding and quotes. Related PRs include [WebUI #48](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/48), [#49](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/49), [#60](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/60), [#63](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/63) and [#65](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/65).
- **Testing and smaller fixes:** Added Foundry and frontend tests and worked on transaction notifications, wallet recipient handling and checking reactor addresses against the factory. The factory address validation change was merged in [WebUI #66](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/66) and wallet recipient handling in [WebUI #67](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/67).
- **Docs and repo work:** Updated the README, deployment notes, privacy and contribution files, `MAINTAINERS.md`, brand files, CI checks and the security review docs. Related PRs include [Contracts #30](https://github.com/StabilityNexus/Gluon-EVM/pull/30), [#45](https://github.com/StabilityNexus/Gluon-EVM/pull/45), [#46](https://github.com/StabilityNexus/Gluon-EVM/pull/46) and [WebUI #37](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/37).
- **Latest testing fixes:** Fixed the net Fusion amount, transmutation quote flicker and the case where an earlier cached quote shows again after changing inputs. I also added the peer audit remediation report. The changes are in [WebUI #69](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/69), [WebUI #68](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/68) and [Contracts #72](https://github.com/StabilityNexus/Gluon-EVM/pull/72).
- **Multichain Factory deployments:** Verified the latest Factory on Ethereum, Polygon, Base, BSC, Ethereum Classic and Sepolia. The contracts deployment record is in [Contracts #74](https://github.com/StabilityNexus/Gluon-EVM/pull/74). The WebUI network setup, historical Sepolia registry support and RPC fixes are in [WebUI #70](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/70), [#71](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/71), [#72](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/72) and [#76](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/76).
- **Live Sepolia verification:** Deployed a new Chainlink-backed Reactor and checked creation, fission, fusion, beta plus and beta minus against transaction events and balances. Seven invalid-operation tests also reverted as expected in read-only simulations.

## Pull Requests

These are my PRs in the three project repositories from May to October 2026, updated through 30 October. The latest additions cover the verified six-network Factory deployments and WebUI RPC fixes. The old WebUI CodeRabbit PR #23 was opened before this report period, so it is not counted.

### Merged pull requests

| Repo | PR | Title | Merged |
|---|---|---|---|
| Gluon-EVM | [#74](https://github.com/StabilityNexus/Gluon-EVM/pull/74) | docs: record verified Factory deployments across six networks | 2026-10-30 |
| Gluon-EVM | [#72](https://github.com/StabilityNexus/Gluon-EVM/pull/72) | docs: add peer audit remediation report | 2026-10-27 |
| Gluon-EVM | [#70](https://github.com/StabilityNexus/Gluon-EVM/pull/70) | chore: align Solidity SPDX headers with interim deployment guidance | 2026-10-25 |
| Gluon-EVM | [#67](https://github.com/StabilityNexus/Gluon-EVM/pull/67) | feat: supply reactor dev settings from factory | 2026-10-25 |
| Gluon-EVM | [#63](https://github.com/StabilityNexus/Gluon-EVM/pull/63) | feat: let deployers choose critical and initial reserve ratios | 2026-10-21 |
| Gluon-EVM | [#62](https://github.com/StabilityNexus/Gluon-EVM/pull/62) | fix: round protocol calculations in favor of reserves | 2026-10-25 |
| Gluon-EVM | [#61](https://github.com/StabilityNexus/Gluon-EVM/pull/61) | fix: limit critical reserve ratio to initial target | 2026-10-18 |
| Gluon-EVM | [#59](https://github.com/StabilityNexus/Gluon-EVM/pull/59) | fix: enforce minimum output for transmutations | 2026-10-19 |
| Gluon-EVM | [#58](https://github.com/StabilityNexus/Gluon-EVM/pull/58) | feat: separate dev fees and add two-step dev transfer | 2026-10-18 |
| Gluon-EVM | [#57](https://github.com/StabilityNexus/Gluon-EVM/pull/57) | docs: explain intentional oracle staleness policy | 2026-10-16 |
| Gluon-EVM | [#56](https://github.com/StabilityNexus/Gluon-EVM/pull/56) | fix: track directional volume in unsigned counters | 2026-10-16 |
| Gluon-EVM | [#55](https://github.com/StabilityNexus/Gluon-EVM/pull/55) | Fix/peg factor after beta | 2026-10-14 |
| Gluon-EVM | [#54](https://github.com/StabilityNexus/Gluon-EVM/pull/54) | Fix/immutable beta parameters main | 2026-10-14 |
| Gluon-EVM | [#47](https://github.com/StabilityNexus/Gluon-EVM/pull/47) | ci: enforce zero Foundry lint warnings | 2026-10-08 |
| Gluon-EVM | [#46](https://github.com/StabilityNexus/Gluon-EVM/pull/46) | docs: record current Orb and Chainlink Sepolia deployments | 2026-10-08 |
| Gluon-EVM | [#45](https://github.com/StabilityNexus/Gluon-EVM/pull/45) | Docs/publish security review | 2026-10-08 |
| Gluon-EVM | [#44](https://github.com/StabilityNexus/Gluon-EVM/pull/44) | docs: add Gluon-EVM privacy policy | 2026-10-08 |
| Gluon-EVM | [#43](https://github.com/StabilityNexus/Gluon-EVM/pull/43) | feat: integrate Orb oracle with cached fallback | 2026-10-04 |
| Gluon-EVM | [#42](https://github.com/StabilityNexus/Gluon-EVM/pull/42) | feat: integrate Orb oracle with cached fallback | 2026-10-02 |
| Gluon-EVM | [#41](https://github.com/StabilityNexus/Gluon-EVM/pull/41) | refactor: simplify reserve naming | 2026-10-02 |
| Gluon-EVM | [#40](https://github.com/StabilityNexus/Gluon-EVM/pull/40) | refactor: clarify reactor accounting conversion boundaries | 2026-10-01 |
| Gluon-EVM | [#39](https://github.com/StabilityNexus/Gluon-EVM/pull/39) | fix: harden reactor reserve accounting | 2026-09-28 |
| Gluon-EVM | [#37](https://github.com/StabilityNexus/Gluon-EVM/pull/37) | refactor: simplify reserve accounting initialization | 2026-09-25 |
| Gluon-EVM | [#36](https://github.com/StabilityNexus/Gluon-EVM/pull/36) | feat: add alpha-based peg adjustment | 2026-09-08 |
| Gluon-EVM | [#34](https://github.com/StabilityNexus/Gluon-EVM/pull/34) | ci: add OpenSSF Scorecard workflow | 2026-08-24 |
| Gluon-EVM | [#33](https://github.com/StabilityNexus/Gluon-EVM/pull/33) | Ci checklist score | 2026-08-24 |
| Gluon-EVM | [#32](https://github.com/StabilityNexus/Gluon-EVM/pull/32) | docs: add Gluon brand assets and guidelines | 2026-08-24 |
| Gluon-EVM | [#31](https://github.com/StabilityNexus/Gluon-EVM/pull/31) | chore: customize CodeRabbit reviews | 2026-08-11 |
| Gluon-EVM | [#30](https://github.com/StabilityNexus/Gluon-EVM/pull/30) | docs: add repository guidelines and documentation | 2026-08-11 |
| Gluon-EVM | [#29](https://github.com/StabilityNexus/Gluon-EVM/pull/29) | refactor: replace oracle min/max reads with interval | 2026-08-11 |
| Gluon-EVM | [#28](https://github.com/StabilityNexus/Gluon-EVM/pull/28) | refactor: replace revert strings with custom errors | 2026-07-30 |
| Gluon-EVM | [#27](https://github.com/StabilityNexus/Gluon-EVM/pull/27) | test: add generic IOracle reactor integration coverage | 2026-07-23 |
| Gluon-EVM | [#25](https://github.com/StabilityNexus/Gluon-EVM/pull/25) | refactor: align IOracle read function names | 2026-07-08 |
| Gluon-EVM | [#24](https://github.com/StabilityNexus/Gluon-EVM/pull/24) | test: add IOracle reactor integration tests | 2026-07-03 |
| Gluon-EVM | [#23](https://github.com/StabilityNexus/Gluon-EVM/pull/23) | Week4 5 ioracle cleanup tests | 2026-06-25 |
| Gluon-EVM | [#22](https://github.com/StabilityNexus/Gluon-EVM/pull/22) | refactor: oracle interface and Chainlink adapter | 2026-06-18 |
| Gluon-EVM-WebUI | [#76](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/76) | fix: replace unavailable Polygon and ETC RPC endpoints | 2026-10-30 |
| Gluon-EVM-WebUI | [#72](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/72) | feat: configure verified Gluon factories across six networks | 2026-10-30 |
| Gluon-EVM-WebUI | [#71](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/71) | fix: add Ethereum mainnet RPC fallback | 2026-10-29 |
| Gluon-EVM-WebUI | [#70](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/70) | feat: enable Ethereum mainnet factory and Chainlink adapter | 2026-10-28 |
| Gluon-EVM-WebUI | [#69](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/69) | fix(ui): deliver requested net amount in ERC20 fusion | 2026-10-27 |
| Gluon-EVM-WebUI | [#68](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/68) | Fix/transmutation quote refresh flicker | 2026-10-27 |
| Gluon-EVM-WebUI | [#67](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/67) | fix: sync default recipient on wallet switch | 2026-10-26 |
| Gluon-EVM-WebUI | [#66](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/66) | fix: verify reactor addresses before enabling interactions | 2026-10-25 |
| Gluon-EVM-WebUI | [#65](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/65) | feat: improve reactor dashboard and creation interface | 2026-10-21 |
| Gluon-EVM-WebUI | [#64](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/64) | feat: support deployer-selected reserve ratios | 2026-10-21 |
| Gluon-EVM-WebUI | [#63](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/63) | feat: replace RainbowKit with WalletLink | 2026-10-21 |
| Gluon-EVM-WebUI | [#61](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/61) | fix: restore transaction notifications and preserve quote precision | 2026-10-21 |
| Gluon-EVM-WebUI | [#60](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/60) | fix: match reserve-favouring contract rounding | 2026-10-20 |
| Gluon-EVM-WebUI | [#58](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/58) | fix: submit quoted transmutation output as on-chain minimum | 2026-10-18 |
| Gluon-EVM-WebUI | [#57](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/57) | fix: align reactor event ABI with dev fee payments | 2026-10-18 |
| Gluon-EVM-WebUI | [#56](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/56) | feat: support separate dev fees in reactor creation and transactions | 2026-10-17 |
| Gluon-EVM-WebUI | [#54](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/54) | Fix/reactor contract pricing | 2026-10-14 |
| Gluon-EVM-WebUI | [#53](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/53) | feat: choose immutable beta settings during reactor creation | 2026-10-16 |
| Gluon-EVM-WebUI | [#52](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/52) | fix: serve WebUI and SEO files from custom domain root | 2026-10-13 |
| Gluon-EVM-WebUI | [#51](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/51) | feat: integrate AOSSIE social sharing | 2026-10-10 |
| Gluon-EVM-WebUI | [#50](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/50) | feat: add static sitemap and page metadata | 2026-10-10 |
| Gluon-EVM-WebUI | [#49](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/49) | refactor: split reactor interaction into components and hooks | 2026-10-10 |
| Gluon-EVM-WebUI | [#48](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/48) | fix: use frontend-driven native asset flow | 2026-10-09 |
| Gluon-EVM-WebUI | [#45](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/45) | Feat/native asset UI | 2026-10-07 |
| Gluon-EVM-WebUI | [#44](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/44) | docs: update Best Practices response evidence | 2026-10-05 |
| Gluon-EVM-WebUI | [#42](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/42) | feat: update Sepolia deployment flow and record Orb/Chainlink deployments | 2026-10-05 |
| Gluon-EVM-WebUI | [#40](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/40) | feat: add direct Orb oracle support to WebUI | 2026-10-04 |
| Gluon-EVM-WebUI | [#38](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/38) | feat: finalize WebUI redesign, static Pages, Terms and privacy | 2026-10-03 |
| Gluon-EVM-WebUI | [#37](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/37) | Docs/webui repository standards | 2026-10-02 |
| Gluon-EVM-WebUI | [#33](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/33) | feat: add oracle preflight validation | 2026-09-08 |
| Gluon-EVM-WebUI | [#31](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/31) | Fix/explorer network | 2026-08-30 |
| Gluon-EVM-WebUI | [#30](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/30) | fix: align reactor interactions with generic oracle | 2026-08-23 |
| Gluon-EVM-WebUI | [#29](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/29) | feat: add Chainlink provider to reactor creation | 2026-08-23 |
| Gluon-EVM-WebUI | [#28](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/28) | feat: update creation for generic oracle adapters | 2026-08-18 |
| OrbOracle-Solidity | [#11](https://github.com/StabilityNexus/OrbOracle-Solidity/pull/11) | docs: explain oracle value ranges | 2026-07-28 |

### Open pull requests

None.

### Closed without merging

| Repo | PR | Title | Reason or what happened next |
|---|---|---|---|
| Gluon-EVM | [#26](https://github.com/StabilityNexus/Gluon-EVM/pull/26) | add OrbOracle adapter | The separate adapter approach was dropped. Orb was later integrated directly using `IOracle`. |
| Gluon-EVM | [#35](https://github.com/StabilityNexus/Gluon-EVM/pull/35) | test: add direct Orb oracle integration | The direct Orb integration was taken forward in the later merged contract work. This PR itself was not merged. |
| Gluon-EVM | [#49](https://github.com/StabilityNexus/Gluon-EVM/pull/49) | feat: add native asset helper for fission and fusion | The helper contract was not used. Native wrapping and unwrapping was handled in the frontend instead. |
| OrbOracle-Solidity | [#14](https://github.com/StabilityNexus/OrbOracle-Solidity/pull/14) | test: Verify direct Gluon integration | This cross-repository test PR was closed without merging. The Gluon side integration continued separately. |

### Issues opened

Most of the issues I worked on during GSoC came up through development, testing and discussions with the mentors. I mainly discussed them on Discord or directly in pull requests instead of creating separate GitHub issues. During Sepolia testing, I found and fixed problems with wallet switching, quote refresh, Fusion amounts and reactor address validation. I also followed existing issues and review comments in the repositories, made the required changes and added tests to verify the fixes.

I also opened [WebUI issue #75](https://github.com/StabilityNexus/Gluon-EVM-WebUI/issues/75) to track unresolved WalletConnect transitive dependency advisories found during multichain validation. The issue is open; its existence does not mean those advisories are fixed.

### Code reviews and peer review work

- I worked through maintainer reviews and CodeRabbit comments on the contract and frontend PRs and made follow-up changes when needed.
- I checked Solidity and frontend behavior during Sepolia testing. Some of the bugs found from this testing were fixed in [WebUI #66](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/66), [#67](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/67), [#68](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/68) and [#69](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/69).
- I also went through the peer audit observations in [Gluon-EVM #71](https://github.com/StabilityNexus/Gluon-EVM/pull/71) and prepared the response in [#72](https://github.com/StabilityNexus/Gluon-EVM/pull/72). The beta minus reserve ratio behavior is part of the current design and the unverified reactor URL issue was fixed in WebUI #66.

### Stats

| Repository | PRs opened | Merged | Open | Closed without merge |
|---|---:|---:|---:|---:|
| Gluon-EVM | 39 | 36 | 0 | 3 |
| Gluon-EVM-WebUI | 34 | 34 | 0 | 0 |
| OrbOracle-Solidity | 2 | 1 | 0 | 1 |
| **Total** | **75** | **71** | **0** | **4** |

These totals are based on the GitHub PR records checked on 30 October 2026. The four PRs closed without merging are listed separately above. The totals include smaller fixes, tests and documentation PRs too.

## Demo

I used Sepolia for the demo and testing. The contract and WebUI deployment documents show the Orb and Chainlink reactors with their addresses and fission and fusion transactions. I also used the browser wallet to check the amounts and the transaction flow.

- **Testnet deployments and earlier transaction results:** [Gluon-EVM Deployments.md](https://github.com/StabilityNexus/Gluon-EVM/blob/main/Deployments.md) and [Gluon-EVM-WebUI Deployments.md](https://github.com/StabilityNexus/Gluon-EVM-WebUI/blob/main/Deployments.md).
- **Recent Fusion transaction:** [Sepolia Etherscan transaction](https://sepolia.etherscan.io/tx/0x48d6c939e9fffc92ed32de86eaa1e999ebf6fd16c57208c6a75e21a474093c29). The wallet received `0.002 dETH` in this test.
- **Gluon Launch Video:** [Watch the video](https://drive.google.com/file/d/1CaT8oX_xKtSoXEXsd9C-0TedR4tJVYdU/view?usp=sharing) — I created this video to introduce Gluon and give a brief overview of how the protocol works.


### Latest Sepolia end-to-end tests (30 October 2026)

I deployed a new Chainlink-backed `Gluon Sepolia E2E Test` Reactor with a `0.001 dETH` seed deposit through the live WebUI. The Reactor is [0xa57813D715919bd6D1d3124F9E9ABeeC0B21Ee5D](https://sepolia.etherscan.io/address/0xa57813D715919bd6D1d3124F9E9ABeeC0B21Ee5D). It uses the verified Sepolia Factory and the existing ETH/USD oracle adapter.

| Operation | Sepolia evidence | Verified result |
|---|---|---|
| Reactor creation | [Transaction](https://sepolia.etherscan.io/tx/0x636c93caa55292268e3bd867838d615895479d923123daefd62c0f5149c1adfc) | Deployed and registered; `0.000999 dETH` held after the developer fee, with initial Neutron and Proton supplies locked in the Reactor |
| Fission | [Transaction](https://sepolia.etherscan.io/tx/0x7ac1b0413c9c0a96f948f3d05b272224614bd8a0a3e64e1fa22b9302f75ddb16) | `0.0001 dETH` supplied; `0.14899086 TGUSD` and `0.0000333 TGPETH` received; reserve increased by `0.0000999 dETH` |
| Fusion | [Transaction](https://sepolia.etherscan.io/tx/0x90e42f4dd8740492d9b088ca6e11bbd58a57d7cfeea2afe9db36ecc6ca42cfd3) | TGUSD and TGPETH burned; `0.00002 dETH` returned; reserve and developer-fee accounting matched |
| Beta plus (Proton to Neutron) | [Transaction](https://sepolia.etherscan.io/tx/0xfee1e8b4280123702731fecfa34f254efa37e3df0a8ed73e2262aebe7c057c2d) | `0.000001 TGPETH` burned and `0.002234862899999996 TGUSD` minted; accounting matched |
| Beta minus (Neutron to Proton) | [Transaction](https://sepolia.etherscan.io/tx/0xbeb2c4e0359907bdf7836500885e3b79a695a3a6d8c1cc4a9ddc2dc3072926be) | `0.001 TGUSD` burned and `0.00000044656027893 TGPETH` minted; accounting matched |

An additional `0.00002 dETH` fission was also completed while testing the WebUI direction controls ([transaction](https://sepolia.etherscan.io/tx/0xd8cc806c7255283f5b6f5efeb4ae7116cef33a9bb2b59760f537a96c2f342a7b)). All five main operations were confirmed using successful receipts and on-chain balances, not only UI notifications. The Reactor's recorded reserve matched its actual token holdings in the verified snapshots.

I also ran seven read-only negative simulations against the deployed Reactor. All seven correctly rejected the tested cases: zero amounts in fission, fusion and both beta directions; an unauthorized call to `initialFission`; and impossible minimum-output requirements in both beta directions. This covers those cases, not every possible security issue.

One small UI limitation observed during the tests is that tiny positive Proton outputs can appear as `0` in the rounded fission preview even though the token is minted correctly. The exact on-chain balance confirmed the positive output.


## Current State of the Project

The contracts support the generic `IOracle` flow, the Chainlink adapter and OrbOracle directly. The frontend has reactor creation, an explorer, wallet connection, fission, fusion, beta transmutation, reserve information and the oracle checks before deployment. The browser sends transactions to the contracts, so it does not need a separate backend for these flows.

For contracts, the test suite runs with Foundry. For the WebUI, I use TypeScript, ESLint, Vitest and the Next.js build. After the multichain changes, all 236 frontend tests across 25 files passed locally, together with TypeScript, ESLint, the production build and performance checks. The merged PRs also passed their GitHub checks. These are checks of the tested code, not a security certificate for mainnet.

As of 30 October, the latest Factory is deployed and verified on Ethereum, Polygon, Base, BSC and Ethereum Classic, along with a new Factory on Sepolia. The WebUI has the verified network addresses, keeps historical Sepolia Reactors accessible, and uses working RPC endpoints for Polygon and ETC. The new Factory deployments did not themselves deploy reserve tokens or Reactor markets on every chain. The end-to-end transaction tests above were on Sepolia; real-funds functionality across all five mainnets has not been proven by this test record.

The production dependency audit still reports 29 unresolved findings (1 critical, 16 high and 12 moderate), with some packages recorded as existing audit exceptions. [WebUI issue #75](https://github.com/StabilityNexus/Gluon-EVM-WebUI/issues/75) tracks specific WalletConnect transitive advisories. Passing the current audit gate does not mean those vulnerabilities have been fixed. I would not treat the frontend and contracts as fully reviewed for real-funds production use on this evidence alone.

### How to try it locally

Contract tests:

```bash
git clone --recurse-submodules https://github.com/StabilityNexus/Gluon-EVM.git
cd Gluon-EVM
forge build
forge test
```

WebUI checks, from the same parent folder:

```bash
cd ..
git clone https://github.com/StabilityNexus/Gluon-EVM-WebUI.git
cd Gluon-EVM-WebUI
npm ci
npm run typecheck
npm run lint
npx vitest run
npm run build
npm run dev
```

To try transactions, use a Sepolia wallet and the current test addresses from the deployment docs. Do not treat the testnet contracts as mainnet contracts.

## Future Work / Next Steps

- Continue maintainer review and security testing before describing the protocol as production-ready for real funds. The verified six-network Factory deployment and Sepolia end-to-end tests are complete.
- Exercise the creation and financial flows on each intended mainnet only after reviewing costs, token and oracle choices, wallet recipients and failure handling; a deployed Factory alone does not establish the full flow.
- Investigate and remediate the outstanding frontend dependency findings, including [WebUI issue #75](https://github.com/StabilityNexus/Gluon-EVM-WebUI/issues/75), and recheck wallet connections and network switching after any dependency update.
- Improve handling of very small quoted token amounts in the UI and continue testing oracle failures, operating-range boundaries and other negative cases without changing the intended protocol design.

## Challenges Faced

The oracle interface changed a few times while I was doing the work. At first I was thinking about different adapters for each oracle, but later Orb moved to the direct `IOracle` interface. I had to go back and change tests and frontend code that were based on the older setup. It took some time, but this way the same reactor code can work with the different oracles.

Another problem was with the pricing and token amounts. Some tokens use different decimals, and there were cases where rounding or fee calculations in the frontend did not match the contracts. I checked the contract functions, made small test transactions on Sepolia, compared the amounts and added tests for the issues I found. The contract size was also very close to the limit, so I had to be careful before adding more Solidity code.

During the multichain browser checks, the Polygon and Ethereum Classic explorer requests failed because the configured public RPC endpoints were unavailable. I tested replacement endpoints with direct Factory reads, fixed the WebUI configuration in PR #76 and confirmed that both networks correctly displayed empty registries afterwards.

## Learnings

I learned a lot more about Solidity and Foundry testing while doing this. Before making a change, I now check which contract functions it affects and what the frontend expects from them. I also got more practice with Next.js, wagmi and viem, especially with how wallet transactions work and how the UI should show errors and pending states.

I also learned that a passing unit test is not always enough. When I started trying the flows with actual Sepolia transactions, I found problems with quote refresh, wallet switching and the Fusion amount. So now I try to check the tests and the real transaction output together when possible.

## My experience

This was a good experience for me because I worked on the same project for a long time and got to understand both the Solidity and frontend parts. Bruno, Sarthak and Aditya helped me with the protocol details and reviewed the decisions when something was not clear. I also had to change some work after discussions, which is normal when more people are working on the same project.

The reviews took time sometimes, but I understood why it is important to keep PRs small and explain what was actually changed. I want to continue working on Gluon and help with mainnet validation and the remaining checks.

## Resources & References

- [Official GSoC 2026 Gluon project page](https://summerofcode.withgoogle.com/programs/2026/projects/l3AkqB4t)
- [AOSSIE GSoC 2026 project listing](https://summerofcode.withgoogle.com/programs/2026/organizations/aossie)
- [Gluon-EVM](https://github.com/StabilityNexus/Gluon-EVM)
- [Gluon-EVM-WebUI](https://github.com/StabilityNexus/Gluon-EVM-WebUI)
- [OrbOracle-Solidity](https://github.com/StabilityNexus/OrbOracle-Solidity)
- [Contracts maintainers file](https://github.com/StabilityNexus/Gluon-EVM/blob/main/MAINTAINERS.md)
- [WebUI maintainers file](https://github.com/StabilityNexus/Gluon-EVM-WebUI/blob/main/MAINTAINERS.md)
- [Contract deployment notes](https://github.com/StabilityNexus/Gluon-EVM/blob/main/Deployments.md)
- [WebUI deployment notes](https://github.com/StabilityNexus/Gluon-EVM-WebUI/blob/main/Deployments.md)
- [Peer audit report PR #71](https://github.com/StabilityNexus/Gluon-EVM/pull/71)
- [Peer audit remediation PR #72](https://github.com/StabilityNexus/Gluon-EVM/pull/72)
- [Six-network Factory deployment documentation PR #74](https://github.com/StabilityNexus/Gluon-EVM/pull/74)
- [Multichain WebUI PR #72](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/72)
- [Polygon and ETC RPC fix PR #76](https://github.com/StabilityNexus/Gluon-EVM-WebUI/pull/76)
- [Open WebUI dependency-security tracking issue #75](https://github.com/StabilityNexus/Gluon-EVM-WebUI/issues/75)

