# Euler Lending Skills

Progressive disclosure skills for the Euler Lending protocol - covering the Ethereum Vault Connector (EVC), Euler Vault Kit (EVK), modular oracle system, and periphery infrastructure.

## Installation

Add this plugin to your Claude Code configuration:

```json
{
  "plugins": ["euler-lending"]
}
```

## Skills

### Core Infrastructure

| Skill | Description | Trigger Keywords |
|-------|-------------|------------------|
| `euler-evc` | Ethereum Vault Connector - sub-accounts, collateral/controller, batch operations, permits | EVC, sub-account, collateral, controller, batch, permit, status check |
| `euler-evk-architecture` | EVault modular architecture - dispatch pattern, modules, storage layout | EVK, EVault, modular, dispatch, vault modules |

### Vault Operations

| Skill | Description | Trigger Keywords |
|-------|-------------|------------------|
| `euler-evk-vault` | ERC-4626 vault operations - deposit, withdraw, mint, redeem, share conversion | deposit, withdraw, mint, redeem, eToken, shares, ERC-4626 |
| `euler-evk-borrowing` | Borrowing operations - borrow, repay, pullDebt, interest accrual, DToken | borrow, repay, debt, pullDebt, dToken, interest accrual |
| `euler-evk-liquidation` | Liquidation mechanics - health checks, discount calculation, collateral seizure | liquidate, liquidation, unhealthy, violator, seize, discount |
| `euler-evk-risk` | Risk configuration - LTV, caps, IRM, oracle integration | LTV, collateral factor, supply cap, borrow cap, IRM, interest rate model |

### Supporting Infrastructure

| Skill | Description | Trigger Keywords |
|-------|-------------|------------------|
| `euler-oracle` | Modular oracle system - EulerRouter, Chainlink/Uniswap/Pyth adapters, bid/ask spreads | oracle, EulerRouter, price feed, Chainlink, Uniswap TWAP, Pyth, adapter |
| `euler-periphery` | Peripheral infrastructure - Perspectives, Lens, Swapper, IRM Factory, Governor | Perspective, vault validation, Lens, Swapper, IRM Factory, Governor |

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    EULER LENDING STACK                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                      PERIPHERY                            │  │
│  │  Perspectives │ Lens │ Swaps │ IRM Factory │ Governor     │  │
│  └───────────────────────────────────────────────────────────┘  │
│                              │                                  │
│  ┌───────────────────────────▼───────────────────────────────┐  │
│  │                         EVK                               │  │
│  │  ┌─────────────────────────────────────────────────────┐  │  │
│  │  │                    EVault                           │  │  │
│  │  │   (ERC-4626 + Borrowing via Modular Dispatch)       │  │  │
│  │  │                                                     │  │  │
│  │  │  Modules: Token │ Vault │ Borrowing │ Liquidation   │  │  │
│  │  │           RiskManager │ Governance │ Initialize     │  │  │
│  │  └─────────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────────┘  │
│                              │                                  │
│  ┌───────────────────────────▼───────────────────────────────┐  │
│  │                   ORACLE LAYER                            │  │
│  │  EulerRouter ─► Chainlink │ Uniswap │ Pyth │ Chronicle    │  │
│  └───────────────────────────────────────────────────────────┘  │
│                              │                                  │
│  ┌───────────────────────────▼───────────────────────────────┐  │
│  │                         EVC                               │  │
│  │  Sub-accounts (256/owner) │ Collateral/Controller Mgmt    │  │
│  │  Deferred Status Checks │ Batch Operations │ Permits      │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Key Concepts

### EVC (Ethereum Vault Connector)
- **Sub-accounts**: Each address gets 256 sub-accounts via XOR encoding
- **Collateral**: Vaults holding user's collateral assets
- **Controller**: Lending vault that can control collateral (for liquidations)
- **Deferred Checks**: Status checks deferred until batch end for gas efficiency

### EVK (Euler Vault Kit)
- **ERC-4626**: Standard vault interface for deposits/withdrawals
- **Modular Dispatch**: Functions delegatecall to specialized modules
- **DToken**: Separate ERC20-like token representing debt
- **Interest Accumulator**: Continuous interest accrual mechanism

### Oracle System
- **EulerRouter**: Central coordinator routing to appropriate adapters
- **Adapters**: Chainlink, Uniswap V3 TWAP, Pyth, Chronicle, Redstone
- **Bid/Ask**: Support for spread-aware pricing

## Source Repositories

This plugin covers multiple Euler repositories:

- [ethereum-vault-connector](https://github.com/euler-xyz/ethereum-vault-connector) - EVC foundation
- [euler-vault-kit](https://github.com/euler-xyz/euler-vault-kit) - Modular vault system
- [euler-price-oracle](https://github.com/euler-xyz/euler-price-oracle) - Oracle infrastructure
- [evk-periphery](https://github.com/euler-xyz/evk-periphery) - Supporting contracts
- [evc-playground](https://github.com/euler-xyz/evc-playground) - Examples and demos

## License

GPL-2.0 (matching Euler protocol license)
