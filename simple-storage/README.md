# simple-storage

Minimal Foundry example: a SimpleStorage contract that stores a favorite number, allows retrieval, and maintains a list of people with their favorite numbers.

## Files of interest
- `src/SimpleStorage.sol` — the Solidity contract.
- `script/DeploySimpleStorage.s.sol` — Foundry script to deploy the contract.
- `foundry.toml` — Foundry config.

## Requirements
- Install Foundry (forge & cast): https://foundry.paradigm.xyz/
- A recent Rust toolchain (for Foundry installation) and standard Unix tooling

## Quick commands
- Build / compile
```bash
forge build
```

- Run tests (if any)
```bash
forge test
```

- Deploy locally with broadcast (creates JSON in `broadcast/`)
```bash
forge script script/DeploySimpleStorage.s.sol:DeploySimpleStorage --rpc-url <RPC_URL> --broadcast
```

## Notes
- This repository ignores build artifacts and broadcasts (see `.gitignore`) so only sources and config are committed.
- If you plan to publish to GitHub, add a remote and push after creating an initial commit.

## License
MIT
