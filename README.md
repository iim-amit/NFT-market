# NFT Marketplace — Sepolia + The Graph

A decentralized NFT marketplace example demonstrating end-to-end Web3 development:
smart contracts (ERC-721), deployment with Hardhat to Sepolia, event indexing with The Graph,
and a Next.js frontend that queries a subgraph via GraphQL.

This README is organized to be actionable and interactive — follow the Quick Start checklist
below and run the exact commands shown.

---

## Table of contents

- [What this repo contains](#what-this-repo-contains)
- [Quickstart (1‑2‑3)](#quickstart-1-2-3)
- [Project structure](#project-structure)
- [Environment (.env) example](#environment-env-example)
- [Deploy contracts (Sepolia)](#deploy-contracts-sepolia)
- [Build & deploy subgraph (The Graph Studio)](#build--deploy-subgraph-the-graph-studio)
- [Frontend: configure & run](#frontend-configure--run)
- [Run tests & local dev node](#run-tests--local-dev-node)
- [GraphQL examples](#graphql-examples)
- [Troubleshooting & notes](#troubleshooting--notes)
- [Contributing and license](#contributing-and-license)

---

## What this repo contains

- Solidity smart contracts:
  - `NftMarketplace.sol` — list, buy, cancel marketplace logic
  - `BasicNft.sol`, `BasicNftTwo.sol` — example ERC721 mintable contracts
- Hardhat setup: tests, scripts, and `hardhat.config.js`
- Subgraph manifest and mappings to index marketplace events
- Frontend (Next.js + Apollo) to interact with contracts and the subgraph

---

## Quickstart (1‑2‑3)

1. Clone the repo
   ```bash
   git clone https://github.com/iim-amit/NFT-market.git
   cd NFT-market
   ```

2. Install dependencies
   ```bash
   # contracts
   cd contracts
   npm install

   # subgraph
   cd ../subgraph
   yarn install

   # frontend (optional)
   cd ../frontend
   yarn install
   ```
   (You can use `npm` instead of `yarn` where you prefer.)

3. Configure environment, deploy contracts, deploy subgraph, then run the frontend.
   Follow the detailed steps below.

---

## Project structure

- contracts/ — Solidity smart contracts, deployments, tests, Hardhat config
- subgraph/ — `schema.graphql`, `subgraph.yaml`, ABIs and mappings
- frontend/ — Next.js UI (queries subgraph and interacts with contracts)
- package.json, lockfiles, README.md

---

## Environment (.env) example

Create a `.env` inside `contracts/` (and `frontend/` if required):

```
SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOUR_ALCHEMY_KEY
PRIVATE_KEY=0xYOUR_WALLET_PRIVATE_KEY
ETHERSCAN_API_KEY=YOUR_ETHERSCAN_KEY
# For frontend:
NEXT_PUBLIC_SUBGRAPH_URL=https://api.studio.thegraph.com/query/...
```

Important:
- Use a testnet-only private key for development — do NOT use mainnet keys with real funds.
- Ensure the RPC provider supports Sepolia (Alchemy, Infura, etc.).

---

## Deploy contracts (Sepolia)

From the `contracts/` folder:

1. Compile & run deployments to Sepolia:
   ```bash
   npx hardhat compile
   npx hardhat deploy --network sepolia
   ```
2. After deployment, copy the deployed contract address(es). You will need:
   - NftMarketplace address (for subgraph `source.address`)
   - startBlock (the block number at which your contract was deployed)

Tip: Hardhat-deploy logs or the deployment script output will include addresses and block numbers.

---

## Build & deploy subgraph (The Graph Studio)

1. Update `subgraph/subgraph.yaml`:
   - Set `source.address` to your deployed NftMarketplace address.
   - Set `startBlock` to the block where the contract was deployed.

Example snippet in `subgraph.yaml`:
```yaml
source:
  address: "0xYOUR_DEPLOYED_CONTRACT"
  abi: NftMarketplace
  startBlock: 12345678
```

2. Codegen, build, and deploy (local):
```bash
cd subgraph
yarn codegen
yarn build
```

3. Deploy to The Graph Studio (hosted service):
```bash
npx graph auth --studio YOUR_STUDIO_DEPLOY_KEY
npx graph deploy --studio your-username/nft-marketplace
```

After successful deploy, copy the GraphQL endpoint and set `NEXT_PUBLIC_SUBGRAPH_URL` in the frontend `.env`.

---

## Frontend: configure & run

1. In `frontend/`, ensure `NEXT_PUBLIC_SUBGRAPH_URL` is set (Graph Studio endpoint).
2. Start dev server:
```bash
cd frontend
yarn dev
# then open http://localhost:3000
```

The UI will let you:
- Mint example NFTs (BasicNft / BasicNftTwo)
- List NFTs on the marketplace
- Buy or cancel listings
- View live listing data powered by the subgraph

---

## Run tests & local Hardhat node

- Run tests:
  ```bash
  cd contracts
  npx hardhat test
  ```

- Optional: run a local Hardhat node for faster iterations:
  ```bash
  npx hardhat node
  # In another terminal, deploy to the local network:
  npx hardhat deploy --network localhost
  ```

Note: Local node accounts are fake ETH — do not reuse keys.

---

## GraphQL examples

Query listed items (example):
```graphql
query {
  itemListeds {
    id
    seller
    price
    nftAddress
    tokenId
    timestamp
  }
}
```

Example: fetch marketplace events filtered by seller or token, adjust query fields per `schema.graphql`.

---

## Troubleshooting & tips

- Subgraph shows no data:
  - Ensure `startBlock` <= deployment block.
  - Confirm `subgraph.yaml` uses the correct contract address and ABI.
  - Check The Graph Studio logs for mapping errors.
- Frontend not showing data:
  - Confirm `NEXT_PUBLIC_SUBGRAPH_URL` is correct and publicly accessible.
  - Check browser console and server logs for GraphQL errors.
- Contract verification:
  - Use `ETHERSCAN_API_KEY` and `npx hardhat verify` to verify on Sepolia Etherscan (if supported).

---

## Useful links

- Hardhat docs: https://hardhat.org
- The Graph Studio: https://thegraph.com/studio
- Sepolia faucets: https://faucets.chain.link/
- Ethers.js: https://docs.ethers.io

---

## Contributing

This repo is a learning example. Contributions welcome:
- Improve docs and tutorials
- Add more tests and examples
- Enhance frontend UX

To contribute:
1. Fork the repo
2. Create a branch: `git checkout -b feat/improve-readme`
3. Send a PR describing your changes

---

## License

MIT — see LICENSE file (or add one if missing).

---

## Notes

- This repository was adapted for Sepolia. Make sure your tooling and RPC endpoints support Sepolia.
- The subgraph only indexes events after the configured `startBlock`.

Happy building! If you'd like, I can:
- open a PR applying this README to the repo, or
- generate a shorter README tailored for a one-page project summary.