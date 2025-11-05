# Bitcoin Development Guide

## Technologies

### 1. Stacks - Smart Contracts on Bitcoin

**Overview:** Layer-1 blockchain that settles on Bitcoin for smart contracts.

**Language:** Clarity (decidable, secure)

**Getting Started:**
```bash
# Install Clarinet
curl -L https://github.com/hirosystems/clarinet/releases/latest/download/clarinet-linux-x64.tar.gz | tar xz
sudo mv clarinet /usr/local/bin

# Create project
clarinet new my-project
cd my-project

# Write contract in contracts/
# Test in tests/
clarinet test

# Deploy
clarinet deploy
```

**Resources:**
- https://docs.stacks.co
- https://clarity-lang.org

### 2. Lightning Network - Fast Payments

**Overview:** Layer-2 payment protocol for instant, low-cost Bitcoin transactions.

**Getting Started:**
```bash
# Install LND
curl -O https://github.com/lightningnetwork/lnd/releases/download/v0.17.0/lnd-linux-amd64-v0.17.0.tar.gz
tar -xzf lnd-linux-amd64-v0.17.0.tar.gz

# Start LND
./lnd --bitcoin.active --bitcoin.testnet

# Create wallet
lncli create

# Open channel
lncli openchannel [node_pubkey] [amount]
```

**Resources:**
- https://lightning.engineering
- https://docs.lightning.engineering

### 3. Rootstock (RSK) - EVM on Bitcoin

**Overview:** EVM-compatible smart contract platform secured by Bitcoin.

**Getting Started:**
```bash
# Install Hardhat
npm install --save-dev hardhat

# Create Hardhat project
npx hardhat

# Configure for RSK in hardhat.config.js
module.exports = {
  networks: {
    rsk: {
      url: "https://public-node.rsk.co",
      chainId: 30
    }
  }
};

# Deploy
npx hardhat run scripts/deploy.js --network rsk
```

**Resources:**
- https://dev.rootstock.io
- https://developers.rsk.co
