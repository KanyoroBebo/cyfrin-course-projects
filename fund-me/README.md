# FundMe — Foundry Smart Contract Project

FundMe is a Solidity contract that accepts ETH contributions only if they meet a USD minimum using Chainlink price feeds. It records funders and amounts, lets only the owner withdraw, and includes deployment scripts, Foundry tests, and a local mock price feed for testing.

---

## Project Overview

FundMe implements the following core logic:

- FundMe
	- `fund()`: accepts ETH if the converted USD value (via Chainlink) is >= `MINIMUM_USD`; records sender and amount.
	- `withdraw()` / `cheaperWithdraw()`: owner-only functions that reset funder balances and transfer the contract balance to the owner (the latter is gas-optimized by copying storage to memory).
	- `getVersion()`: returns the Chainlink price feed version.
	- `receive()` / `fallback()`: forward direct ETH transfers to `fund()`.
	- Access control: owner is set at deployment; non-owners are reverted by the `onlyOwner` modifier.

- PriceConverter (library)
	- `getPrice(priceFeed)`: reads `latestRoundData()` from a Chainlink `AggregatorV3Interface` and normalizes the price to 18 decimals.
	- `getConversionRate(ethAmount, priceFeed)`: computes the USD value of an ETH amount using the normalized price.

---

## Tech Stack

- **Solidity** `^0.8.x`
- **Foundry** (Forge, Cast, Anvil)
- **Chainlink Brownie Contracts**
- **Anvil** for local testing
- **Sepolia Testnet** for deployment

---

## Folder Structure

```
fund-me/
├── lib/                       # External dependencies (installed via `forge install`)
├── script/                    # Deployment & helper scripts
│   ├── DeployFundMe.s.sol
│   └── HelperConfig.s.sol
├── src/                       # Solidity contracts
│   ├── FundMe.sol
│   └── PriceConverter.sol
├── test/                      # Tests
│   ├── unit/                  # Unit tests (e.g. FundMeTest.t.sol)
│   └── integration/           # Integration tests (e.g. InteractionsTest.t.sol)
├── broadcast/                 # Forge broadcast outputs (optional)
├── cache/                     # Foundry cache files
├── foundry.toml               # Foundry configuration
├── Makefile                   # Convenience tasks (build/test/deploy)
└── README.md                  # Project documentation


```

---

## Commands (build / test / deploy)

- Install dependencies:
	- `forge install` or `make install`

- Build:
	- `forge build` or `make build`

- Run tests:
	- All tests: `forge test -vvv` or `make test`
	- Single test: `forge test --match-test <TestName>`

- Start a local node:
	- `anvil` or `make anvil`

- Deploy:
	- Local (Anvil): `make deploy` (uses defaults in `Makefile`)
	- Sepolia: `make deploy-sepolia` (ensure `.env` has `SEPOLIA_RPC_URL`, `ACCOUNT`, `ETHERSCAN_API_KEY` if verifying)

- Direct Forge script (example):
	- `forge script script/DeployFundMe.s.sol:DeployFundMe --rpc-url <RPC> --private-key <KEY> --broadcast`

Notes: the `Makefile` wraps common `forge` commands and sets sensible defaults (check `Makefile` for `NETWORK_ARGS` and keys). For local deterministic runs, `script/HelperConfig.s.sol` deploys a `MockV3Aggregator` automatically.
 
---

## Testing 

- Tests use Foundry's `forge` and the `forge-std` library for utilities and cheatcodes.
- Unit tests are located under `test/unit/` (e.g. `FundMeTest.t.sol`) and validate funding, owner access, and withdrawal logic.
- Integration tests are under `test/integration/` (e.g. `InteractionsTest.t.sol`) and exercise script-driven flows.
- Mocks: `MockV3Aggregator` is deployed on local chains by `script/HelperConfig.s.sol` so price feed behavior is deterministic.
- Common cheatcodes used in tests:
	- `vm.prank` / `vm.startPrank` / `vm.stopPrank` — impersonate senders.
	- `vm.deal` / `hoax` — set balances and simulate transactions from arbitrary addresses.
	- `vm.expectRevert` — assert expected reverts.
- Run tests:
	- `forge test -vvv` (all tests)
	- `forge test --match-test <TestName>` (single test)

---

## Environment variables

- `SEPOLIA_RPC_URL` — RPC endpoint for Sepolia (used for remote deploys).
- `ACCOUNT` — Address or account identifier used by the `Makefile` when deploying to remote networks.
- `ETHERSCAN_API_KEY` — Etherscan API key (used when verifying contracts with `--verify`).
- `DEFAULT_ANVIL_KEY` — Default private key used for local Anvil broadcasts in `Makefile` (example key; do NOT use on mainnet).
- `DEFAULT_ZKSYNC_LOCAL_KEY` — Default key used for local zkSync builds/deploys in the `Makefile` (example only).
- `ZKSYNC_SEPOLIA_RPC_URL` — (optional) RPC for zkSync Sepolia if using zk deploy targets.

Notes:
- Keep private keys and API keys out of version control. Use a local `.env` file or CI secrets (GitHub Actions) for automation.
- `script/HelperConfig.s.sol` deploys `MockV3Aggregator` on local chains, so you generally only need RPC and account values for remote deployments.

---

## Security & Access Control

* Only the **owner** can call `withdraw()`.
* The contract rejects direct ETH transfers without using `fund()`.
* Chainlink oracle ensures USD-based validation.

---

## License

MIT License © 2025

---

## Author

**Kevin Kanyoro**
[GitHub Profile](https://github.com/KanyoroBebo)

```