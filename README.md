NFT Marketplace

A decentralized NFT Marketplace built on Ethereum Sepolia testnet with The Graph Studio for indexing and querying blockchain events.

This project demonstrates smart contract development, deployment, subgraph indexing, and frontend interaction using modern Web3 tools.

🧩 Project Features

Deploy ERC721 NFTs (BasicNft, BasicNftTwo)

List, buy, and cancel NFTs in a marketplace (NftMarketplace.sol)

Event indexing with The Graph Studio

Query NFT events via GraphQL API

Hardhat for testing, deployment, and scripts

Sepolia testnet integration

📁 Project Structure
NFT-market/
│
├─ contracts/           # Smart contracts & Hardhat config
│   ├─ NftMarketplace.sol
│   ├─ BasicNft.sol
│   ├─ BasicNftTwo.sol
│   ├─ deploy/           # Deployment scripts
│   ├─ test/             # Tests
│   └─ hardhat.config.js
│
├─ frontend/           
│
├─ subgraph/            # The Graph subgraph
│   ├─ schema.graphql
│   ├─ subgraph.yaml
│   ├─ abis/
│   ├─ src/mapping.ts
│
├─ package.json
├─ yarn.lock / package-lock.json
└─ README.md

⚡ Tech Stack

Solidity – Smart contracts

Hardhat – Development, testing, deployment

Ethers.js – Interacting with blockchain

The Graph Studio – Subgraph indexing & GraphQL queries

React.js / Apollo Client – Frontend (optional)

🔧 Setup & Installation

Clone the repository:

git clone https://github.com/yourusername/NFT-market.git
cd NFT-market


Install dependencies:

cd contracts
npm install

cd ../subgraph
yarn install


Configure .env file in contracts/:

SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOUR_ALCHEMY_KEY
PRIVATE_KEY=0xYOUR_WALLET_PRIVATE_KEY
ETHERSCAN_API_KEY=YOUR_ETHERSCAN_KEY

🛠 Deploy Contracts (Sepolia)
npx hardhat deploy --network sepolia


Copy the deployed contract address for the subgraph configuration.

📈 Configure & Deploy Subgraph

Update subgraph/subgraph.yaml:

source:
  address: "0xYOUR_DEPLOYED_CONTRACT"
  abi: NftMarketplace
  startBlock: YOUR_DEPLOY_BLOCK


Generate types & build subgraph:

cd subgraph
yarn codegen
yarn build


Deploy to The Graph Studio:

npx graph auth --studio YOUR_DEPLOY_KEY
npx graph deploy --studio your-username/nft-marketplace


After deployment, copy the GraphQL endpoint for querying data.

📊 Query Data

Use GraphQL to query NFT events:

{
  itemListeds {
    id
    seller
    price
    nftAddress
    tokenId
  }
}

💻 Run Local Hardhat Node (Optional)
npx hardhat node


Use this for local testing or connecting frontend locally.

✅ Testing

Run all smart contract tests:

npx hardhat test

🔗 Useful Links

Sepolia Etherscan
 – Verify and explore contracts

The Graph Studio
 – Subgraph deployment & queries

Hardhat Docs
 – Smart contract development

📌 Notes

All accounts used in local Hardhat node are fake ETH and should not be used on Mainnet.

Subgraph only indexes on-chain events after startBlock.

Ensure your wallet has Sepolia ETH for deploying contracts.
