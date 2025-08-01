# VaultShare Protocol

[![Stacks](https://img.shields.io/badge/Stacks-2.1-purple)](https://stacks.org)
[![Clarity](https://img.shields.io/badge/Clarity-Smart%20Contract-blue)](https://clarity-lang.org)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen)](https://github.com/barlogun/vault-share)

## Overview

VaultShare Protocol is a next-generation decentralized wealth management platform that leverages Bitcoin's security through the Stacks blockchain to democratize access to premium asset classes via tokenized fractional ownership and automated governance.

The protocol revolutionizes traditional asset management by creating a trustless, transparent ecosystem where high-value real-world assets are transformed into liquid, tradeable digital securities. Built on Stacks L2, VaultShare combines institutional-grade compliance with DeFi innovation.

## Key Features

### 🏦 **Fractional Asset Tokenization**

- Transform high-value assets into liquid digital securities
- Standard tokenization ratio of 100,000 tokens per asset
- Automated ownership tracking and management
- Support for assets valued from $1,000 to $1 trillion

### 🗳️ **Democratic Governance**

- Token-weighted voting mechanisms
- Community-driven asset management decisions
- Transparent proposal creation and execution
- Minimum stake requirements for governance participation

### 💰 **Automated Dividend Distribution**

- Proportional dividend claims based on token holdings
- Real-time claimable amount calculations
- Historical dividend tracking per token holder
- Seamless distribution without intermediaries

### 🔐 **Identity Verification & Compliance**

- Multi-level KYC verification system (Levels 1-5)
- Expiring verification certificates
- Regulatory compliance framework
- Address-based identity management

### 📊 **Oracle Price Integration**

- External price feed integration
- Real-time asset valuation updates
- Decimal precision support
- Oracle-sourced market data

## Technical Architecture

### Smart Contract Structure

```
VaultShare Protocol
├── Core Functions
│   ├── Asset Registration & Tokenization
│   ├── Dividend Distribution System
│   ├── Governance Proposal Management
│   └── Weighted Voting Mechanism
├── Data Storage
│   ├── Asset Registry
│   ├── Token Balance Ledger
│   ├── KYC Status Database
│   ├── Governance Proposals
│   ├── Voting Records
│   ├── Dividend Claims
│   └── Price Feeds
└── Security & Validation
    ├── Input Validation Framework
    ├── Access Control System
    └── Error Handling
```

### Protocol Constants

| Parameter | Value | Description |
|-----------|-------|-------------|
| `MAX-ASSET-VALUE` | 1,000,000,000,000 | Maximum asset value (1 trillion) |
| `MIN-ASSET-VALUE` | 1,000 | Minimum asset value |
| `MAX-DURATION` | 144 blocks | Maximum proposal duration (~24 hours) |
| `MIN-DURATION` | 12 blocks | Minimum proposal duration (~2 hours) |
| `MAX-KYC-LEVEL` | 5 | Maximum KYC verification level |
| `tokens-per-asset` | 100,000 | Standard tokenization ratio |

## Getting Started

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) - Stacks development environment
- [Node.js](https://nodejs.org/) v16+ - For testing framework
- [Stacks CLI](https://docs.stacks.co/stacks-cli) - Command line interface

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/barlogun/vault-share.git
   cd vault-share
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Initialize Clarinet environment**

   ```bash
   clarinet check
   ```

### Project Structure

```
vault-share/
├── contracts/
│   └── vault-share.clar          # Main protocol contract
├── tests/
│   └── vault-share.test.ts       # Comprehensive test suite
├── settings/
│   ├── Devnet.toml              # Development configuration
│   ├── Testnet.toml             # Testnet configuration
│   └── Mainnet.toml             # Mainnet configuration
├── Clarinet.toml                # Clarinet project configuration
├── package.json                 # Node.js dependencies
├── tsconfig.json                # TypeScript configuration
└── vitest.config.js             # Test configuration
```

## Usage Examples

### Asset Registration

```clarity
;; Register a new asset (owner only)
(contract-call? .vault-share register-asset 
  "https://metadata.example.com/asset/1" 
  u50000000)  ;; $50M asset value
```

### Token Balance Query

```clarity
;; Check token balance for an address
(contract-call? .vault-share get-balance 
  'SP1HTBVD3JG9C05J7HBJTHGR0GGW7KX975CN0QKN 
  u1)  ;; Asset ID 1
```

### Creating Governance Proposals

```clarity
;; Create a governance proposal
(contract-call? .vault-share create-proposal 
  u1                                    ;; Asset ID
  "Increase dividend distribution rate" ;; Proposal title
  u144                                  ;; Duration (24 hours)
  u10000)                              ;; Minimum votes required
```

### Voting on Proposals

```clarity
;; Vote on a proposal with token weight
(contract-call? .vault-share vote 
  u1     ;; Proposal ID
  true   ;; Vote for (true) or against (false)
  u5000) ;; Amount of tokens to vote with
```

### Claiming Dividends

```clarity
;; Claim available dividends
(contract-call? .vault-share claim-dividends u1) ;; Asset ID
```

## API Reference

### Public Functions

#### `register-asset`

Registers a new asset for tokenization (owner only).

**Parameters:**

- `metadata-uri`: Asset metadata URI (max 256 characters)
- `asset-value`: Asset value in base units

**Returns:** Asset ID on success

#### `claim-dividends`

Claims available dividends for a token holder.

**Parameters:**

- `asset-id`: Target asset identifier

**Returns:** Success confirmation

#### `create-proposal`

Creates a new governance proposal.

**Parameters:**

- `asset-id`: Target asset for proposal
- `title`: Proposal title (max 256 characters)
- `duration`: Voting duration in blocks
- `minimum-votes`: Required votes for proposal validity

**Returns:** Proposal ID on success

#### `vote`

Submits a weighted vote on a governance proposal.

**Parameters:**

- `proposal-id`: Target proposal identifier
- `vote-for`: Boolean vote direction
- `amount`: Token weight for vote

**Returns:** Success confirmation

### Read-Only Functions

#### `get-asset-info`

Retrieves comprehensive asset information.

#### `get-balance`

Returns token balance for an address and asset.

#### `get-proposal`

Fetches governance proposal details.

#### `get-vote`

Retrieves individual vote information.

#### `get-price-feed`

Accesses oracle price feed data.

#### `get-last-claim`

Returns last dividend claim amount.

## Testing

### Run Contract Validation

```bash
clarinet check
```

### Execute Test Suite

```bash
npm test
```

### Test Coverage

The test suite covers:

- Asset registration and tokenization
- Token balance management
- Governance proposal lifecycle
- Voting mechanisms
- Dividend distribution
- Input validation
- Error handling
- Edge cases and security scenarios

## Security Considerations

### Access Control

- **Owner-only functions**: Asset registration restricted to contract owner
- **Stake requirements**: Governance participation requires minimum token holdings
- **Voting restrictions**: One vote per address per proposal

### Input Validation

- **Value bounds**: Asset values validated against min/max limits
- **Duration limits**: Proposal durations within acceptable ranges
- **URI validation**: Metadata URIs validated for length and format
- **Amount verification**: Token amounts validated against balances

### Error Handling

The protocol implements comprehensive error codes:

- `u100-u109`: Core access and operational errors
- `u110-u117`: Input validation errors

## Deployment

### Testnet Deployment

```bash
clarinet deployments apply -p testnet
```

### Mainnet Deployment

```bash
clarinet deployments apply -p mainnet
```

## Contributing

We welcome contributions to VaultShare Protocol! Please follow these guidelines:

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Commit changes**: `git commit -m 'Add amazing feature'`
4. **Push to branch**: `git push origin feature/amazing-feature`
5. **Open a Pull Request**

### Development Guidelines

- Follow Clarity best practices
- Maintain comprehensive test coverage
- Document all public functions
- Use consistent code formatting
- Include security considerations in design

## Roadmap

### Phase 1: Core Protocol ✅

- [x] Asset tokenization engine
- [x] Fractional ownership system
- [x] Basic governance framework
- [x] Dividend distribution mechanism

### Phase 2: Enhanced Features 🚧

- [ ] Advanced KYC integration
- [ ] Multi-oracle price aggregation
- [ ] Automated compliance reporting
- [ ] Cross-chain asset bridges

### Phase 3: Ecosystem Expansion 📋

- [ ] Institutional partnerships
- [ ] Mobile application
- [ ] Advanced analytics dashboard
- [ ] Decentralized asset management tools

Built with ❤️ on Stacks • Securing the future of decentralized wealth management
