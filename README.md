# FundMe

A decentralized funding contract built with Solidity and Foundry. Users can send ETH to the contract, which tracks donations and allows the owner to withdraw funds. The contract uses Chainlink Price Feeds to ensure minimum donation amounts in USD.

## 🚀 Features

- **ETH Funding**: Accept ETH donations from users
- **Minimum Donation**: Enforces a minimum donation of $5 USD using Chainlink Price Feeds
- **Owner Withdraw**: Contract owner can withdraw all funds
- **Funder Tracking**: Tracks all funders and their contribution amounts
- **Gas Optimized**: Includes optimized withdraw function using storage arrays
- **Multi-Network Support**: Deployable to Ethereum Mainnet, Sepolia, and local Anvil

## 📋 Prerequisites

- [Foundry](https://book.getfoundry.sh/getting-started/installation) (forge, cast, anvil)
- Node.js (for some dependencies)
- Git

## 🔧 Installation

1. Clone the repository:
```bash
git clone <your-repo-url>
cd foundry_fund_me
```

2. Install dependencies:
```bash
forge install
```

3. Set up environment variables (create a `.env` file):
```bash
SEPOLIA_RPC_URL=your_sepolia_rpc_url
PRIVATE_KEY=your_private_key
ETHERSCAN_API_KEY=your_etherscan_api_key
```

## 📁 Project Structure

```
foundry_fund_me/
├── src/
│   ├── FundMe.sol           # Main contract
│   └── PriceConverter.sol   # Library for price conversions
├── script/
│   ├── DeployFundMe.s.sol   # Deployment script
│   ├── HelperConfig.s.sol   # Network configuration helper
│   └── Interactions.s.sol   # Interaction scripts (fund, withdraw)
├── test/
│   ├── unit/
│   │   ├── FundMeTest.t.sol # Unit tests
│   │   └── ZkSyncDevOps.t.sol
│   ├── integration/
│   │   └── InteractionsTest.t.sol # Integration tests
│   └── mocks/
│       └── MockV3Aggregator.sol   # Mock Chainlink price feed
├── lib/                      # Dependencies
├── foundry.toml             # Foundry configuration
└── Makefile                 # Make commands
```

## 🧪 Testing

Run all tests:
```bash
forge test
```

Run with verbose output:
```bash
forge test -vvv
```

Run specific test:
```bash
forge test --match-test testMinDollarIsFive
```

Run tests on a fork:
```bash
forge test --fork-url $SEPOLIA_RPC_URL
```

### Test Coverage

The test suite includes:
- ✅ Minimum donation amount validation
- ✅ Owner functionality
- ✅ Funding and withdrawal
- ✅ Funder tracking
- ✅ Access control (onlyOwner modifier)
- ✅ Integration tests

Generate coverage report:
```bash
forge coverage
```

## 🚀 Deployment

### Deploy to Sepolia Testnet

Make sure your `.env` file is set up with:
- `SEPOLIA_RPC_URL`
- `PRIVATE_KEY`
- `ETHERSCAN_API_KEY`

```bash
make deploy-sepolia
```

Or manually:
```bash
forge script script/DeployFundMe.s.sol:DeployFundMe \
  --rpc-url $SEPOLIA_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --verify \
  --etherscan-api-key $ETHERSCAN_API_KEY \
  --chain sepolia -vvvv
```

### Deploy to Local Anvil

1. Start Anvil in a separate terminal:
```bash
anvil
```

2. Deploy (using the Makefile):
```bash
make deploy
```

Or manually:
```bash
forge script script/DeployFundMe.s.sol:DeployFundMe \
  --rpc-url http://localhost:8545 \
  --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 \
  --broadcast
```

## 📖 Usage

### Funding the Contract

Users can send ETH directly to the contract:

```solidity
// Minimum donation is $5 USD worth of ETH
fundMe.fund{value: ethAmount}();
```

The contract will:
- Check if the ETH amount is worth at least $5 USD
- Record the sender's address and contribution amount
- Add the sender to the funders list

### Withdrawing Funds (Owner Only)

The contract owner can withdraw all funds:

```solidity
fundMe.withdraw();
```

Or use the gas-optimized version:
```solidity
fundMe.cheapWithdraw();
```

### Query Functions

```solidity
// Get minimum USD amount
uint256 minimum = fundMe.MINIMUM_USD();

// Get contract owner
address owner = fundMe.getOwner();

// Get amount funded by an address
uint256 amount = fundMe.getAddressToAmountFunded(address);

// Get funder at index
address funder = fundMe.getFunder(uint256 index);

// Get price feed version
uint256 version = fundMe.getVersion();
```

## 🔗 Network Configuration

The contract uses different Chainlink Price Feed addresses based on the network:

- **Sepolia Testnet**: `0x694AA1769357215DE4FAC081bf1f309aDC325306`
- **Ethereum Mainnet**: `0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419`
- **Anvil (Local)**: Deploys a mock price feed

The `HelperConfig.s.sol` contract automatically detects the network and uses the appropriate configuration.

## 🛠️ Development

### Build
```bash
forge build
```

### Format
```bash
forge fmt
```

### Gas Snapshots
```bash
forge snapshot
```

### Available Make Commands
```bash
make build          # Build the project
make test           # Run tests with verbose output
make deploy-sepolia # Deploy to Sepolia testnet
```

## 📚 Key Contracts

### FundMe.sol
Main contract that handles funding and withdrawals.

**Key Features:**
- Minimum donation enforcement ($5 USD)
- Owner-only withdrawal
- Funder tracking
- Gas-optimized withdraw function

### PriceConverter.sol
Library for converting ETH amounts to USD using Chainlink Price Feeds.

**Functions:**
- `getPrice()`: Gets current ETH/USD price
- `getConversionRate()`: Converts ETH amount to USD value

## 🔒 Security Features

- ✅ Access control (onlyOwner modifier)
- ✅ Minimum donation enforcement
- ✅ Safe withdrawal mechanism using `call()` instead of `transfer()`
- ✅ Proper error handling with custom errors
- ✅ Input validation

## 📝 License

MIT License

## 🙏 Acknowledgments

- [Chainlink](https://chain.link/) for price feed infrastructure
- [Foundry](https://book.getfoundry.sh/) for the development framework
- [Patrick Collins](https://github.com/PatrickAlphaC) for the tutorial

## 📞 Contact

For questions or issues, please open an issue on GitHub.

---

**⚠️ Warning**: This is a learning project. Do not use in production without thorough security audits.
