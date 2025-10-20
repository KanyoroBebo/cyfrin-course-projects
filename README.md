# Cyfrin Solidity & Foundry Course Projects

This repository contains my practice and project work from the **Cyfrin Solidity & Foundry Developer Course**.  
Each folder represents a different module or project built while learning smart contract development using **Foundry**.

---

## 🧩 Tech Stack

- **Solidity** – Smart contract language  
- **Foundry** – Framework for building, testing, and deploying contracts  
- **Anvil** – Local Ethereum node for development  
- **Forge** – Compilation, testing, and deployment tool  
- **Cast** – Interact with deployed contracts via CLI  

---

## 📂 Repository Structure

```
README.md
.gitignore
fund-me/
  foundry.toml
  src/
    Counter.sol
  script/
    Counter.s.sol
  test/
    Counter.t.sol
simple-storage/
  src/
  test/
MyToken/
  MyToken.sol
test/
```

Notes:
- Minimal view: intentionally omits files/folders excluded by `.gitignore` (for example: `cache/`, `out/`, `lib/`, `broadcast/`, `node_modules/`).
- Run `forge build` and `forge test` inside any project folder to build and test that project.
````

---

## ⚙️ Getting Started

1. **Install Foundry**  
   ```bash
   curl -L https://foundry.paradigm.xyz | bash
   foundryup
````

2. **Clone this repository**

   ```bash
   git clone https://github.com/<your-username>/cyfrin-course-projects.git
   cd cyfrin-course-projects
   ```

3. **Run locally with Anvil**

   ```bash
   anvil
   ```

4. **Build and test a project**

   ```bash
   cd simple-storage
   forge build
   forge test
   ```

---

## 🧠 Notes

These projects are part of my ongoing journey to mastering smart contract development and blockchain engineering.
Each project demonstrates key concepts from the course — from basic storage and tokens to testing, deployment, and security.

---

## 🪪 License

MIT License © 2025 Kevin Kanyoro