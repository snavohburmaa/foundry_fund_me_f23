# FundMe

A decentralized funding contract built with Solidity and Foundry. Users can send ETH to the contract, which tracks donations and allows the owner to withdraw funds. The contract uses Chainlink Price Feeds to ensure minimum donation amounts in USD.

##  Features

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

### Test Coverage

The test suite includes:
- ✅ Minimum donation amount validation
- ✅ Owner functionality
- ✅ Funding and withdrawal
- ✅ Funder tracking
- ✅ Access control (onlyOwner modifier)
- ✅ Integration tests

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

The `HelperConfig.s.sol` contract automatically detects the network and uses the appropriate configuration.


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


