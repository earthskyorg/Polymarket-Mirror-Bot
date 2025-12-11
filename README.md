# Polymarket Trading Bot

[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-blue)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-16+-green)](https://nodejs.org/)
[![License](https://img.shields.io/badge/License-ISC-blue)](LICENSE)

A TypeScript-based automated trading bot for Polymarket prediction markets, featuring comprehensive credential management, order execution, market analysis, and automated arbitrage trading capabilities.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Architecture](#architecture)
- [Trading Strategy](#trading-strategy)
- [Development](#development)
- [Security](#security)
- [Documentation](#documentation)
- [License](#license)
- [Disclaimer](#disclaimer)

## Overview

This bot enables automated trading on Polymarket by monitoring price discrepancies between software price oracles and market prices. It supports both manual interactive trading and fully automated arbitrage strategies with built-in risk management.

### Operating Modes

**Manual Trading (Interactive CLI)**
- Interactive command-line interface for manual trade execution
- Real-time market analysis and price monitoring
- Account management and balance checking

**Automated Trading Bot**
- Fully automated arbitrage trading
- Real-time price monitoring via WebSocket connections
- Automatic trade execution with risk management
- Configurable stop loss and take profit levels

## Features

- 🔐 **Credential Management**: Secure private key handling and API authentication
- 💰 **Allowance Control**: Manage USDC token allowances for trading operations
- 📊 **Market Analysis**: Real-time bid/ask spreads and price data retrieval
- 🎯 **Order Execution**: Place market and limit orders with automatic validation
- 🔍 **Market Discovery**: Auto-detect and track current Bitcoin markets
- 📈 **Price Tracking**: Real-time price updates from order books via WebSocket
- 🤖 **Automated Trading**: Fully automated arbitrage trading with configurable risk management
- ⚖️ **Balance Monitoring**: Automatic wallet balance checks before trading
- 🛡️ **Risk Management**: Built-in stop loss and take profit protection
- 🔄 **Error Recovery**: Comprehensive error handling and automatic reconnection

## Quick Start

### Prerequisites

- Node.js (v16 or higher)
- npm or yarn package manager
- Ethereum wallet with private key
- USDC on Polygon network (minimum $5, recommended $50+)
- MATIC for gas fees (minimum 0.05, recommended 0.5+)

### 1. Clone and Install

```bash
git clone <repository-url>
cd polymarket-mirror-bot
npm install
```

### 2. Configure Environment

Create a `.env` file in the project root:

```env
PRIVATE_KEY=your_private_key_here
CLOB_API_URL=https://clob.polymarket.com
POLYGON_CHAIN_ID=137
SOFTWARE_WS_URL=
PRICE_DIFFERENCE_THRESHOLD=0.015
STOP_LOSS_AMOUNT=0.005
TAKE_PROFIT_AMOUNT=0.01
DEFAULT_TRADE_AMOUNT=5.0
TRADE_COOLDOWN=30
```

### 3. Generate Credentials

```bash
npm run gen-creds
```

### 4. Start Trading

**Automated Mode:**
```bash
npm run auto-trade
```

**Manual Mode:**
```bash
npm run dev
```

**Windows (PowerShell):**
```powershell
.\start-bot.ps1
```

## Installation

### System Requirements

- **Node.js**: v16.0.0 or higher
- **npm**: v7.0.0 or higher (or yarn equivalent)
- **Operating System**: Windows, macOS, or Linux

### Step-by-Step Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd polymarket-mirror-bot
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Verify installation**
   ```bash
   npm run build
   ```

## Configuration

### Environment Variables

Create a `.env` file in the project root with the following configuration:

```env
# Wallet Configuration
PRIVATE_KEY=your_private_key_here

# API Configuration
CLOB_API_URL=https://clob.polymarket.com
POLYGON_CHAIN_ID=137

# Auto Trading Parameters
SOFTWARE_WS_URL=
PRICE_DIFFERENCE_THRESHOLD=0.015
STOP_LOSS_AMOUNT=0.005
TAKE_PROFIT_AMOUNT=0.01
DEFAULT_TRADE_AMOUNT=5.0
TRADE_COOLDOWN=30
```

### Configuration Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `PRIVATE_KEY` | string | required | Ethereum wallet private key |
| `CLOB_API_URL` | string | `https://clob.polymarket.com` | Polymarket CLOB API endpoint |
| `POLYGON_CHAIN_ID` | number | `137` | Polygon network chain ID |
| `SOFTWARE_WS_URL` | string | required | Software price oracle WebSocket URL |
| `PRICE_DIFFERENCE_THRESHOLD` | number | `0.015` | Minimum price difference to trigger trade |
| `TAKE_PROFIT_AMOUNT` | number | `0.01` | Profit target above buy price |
| `STOP_LOSS_AMOUNT` | number | `0.005` | Maximum loss below buy price |
| `DEFAULT_TRADE_AMOUNT` | number | `5.0` | USDC amount per trade |
| `TRADE_COOLDOWN` | number | `30` | Seconds between trades |

### Initial Setup

1. **Generate CLOB Credentials** (First Time Only)
   ```bash
   npm run gen-creds
   ```
   This creates API credentials required for authenticated trading operations. Credentials are saved to `.credentials.json` (automatically git-ignored).

2. **Check Wallet Balance**
   ```bash
   npm run check-balance
   ```
   Verify you have sufficient USDC and MATIC before starting the bot.

## Usage

### Automated Trading Mode

Start the fully automated arbitrage trading bot:

```bash
npm run auto-trade
```

**What the bot does:**

1. **Connects to Data Feeds**
   - Software price oracle (WebSocket) for real-time probability calculations
   - Polymarket market feed (WebSocket) for current market prices

2. **Monitors Price Differences**
   - Continuously compares software oracle prices vs. market prices
   - Detects when price difference exceeds configured threshold

3. **Executes Automated Trades**
   - Places market BUY order when opportunity detected
   - Sets take profit limit SELL order (+$0.01 above buy price by default)
   - Sets stop loss limit SELL order (-$0.005 below buy price by default)
   - Waits for cooldown period before next trade

4. **Risk Management**
   - Automatic stop loss protection
   - Take profit locking in gains
   - Configurable trade amounts and thresholds

**Example Output:**
```
Starting Auto Trading Bot...
Wallet: 0xYourAddress...
Finding current Bitcoin market...
✅ Market found: Bitcoin Up or Down - November 22, 9AM ET
✅ Software WebSocket connected
✅ Polymarket WebSocket connected
✅ Bot started successfully!
Monitoring for trade opportunities...

🎯 Trade opportunity detected!
Software Price: $0.7500 | Market Price: $0.7300 | Difference: $0.0200
📊 Executing trade...
✅ Buy order placed
✅ Take Profit order placed at $0.7400
✅ Stop Loss order placed at $0.7250
✅ Trade execution complete!
```

**To Stop:** Press `Ctrl+C` to stop the bot gracefully.

### Manual Interactive Mode

Run the interactive CLI for manual trading:

```bash
npm run dev
```

Provides an interactive menu for:
- Manual trade execution
- Market analysis
- Balance checking
- Order management

### Available Scripts

```bash
# Development
npm run dev              # Run interactive CLI in development mode
npm run build            # Compile TypeScript to JavaScript
npm start                # Run compiled version

# Trading Operations
npm run auto-trade       # Start automated trading bot
npm run check-balance    # Check wallet balances (USDC and MATIC)

# Credentials & Configuration
npm run gen-creds        # Generate CLOB API credentials
npm run credentials      # Display credential information

# Market Operations
npm run market           # Find current Bitcoin market
npm run bid-ask <token>  # Get bid/ask prices for token ID
npm run allowance        # Check and manage USDC allowance
npm run order            # Place orders interactively
```

## Architecture

### Project Structure

```
polymarket-mirror-bot/
├── src/
│   ├── main.ts                  # Interactive CLI trading interface
│   ├── auto_trading_bot.ts      # Automated arbitrage bot
│   ├── balance_checker.ts       # Wallet balance checking
│   ├── check_balance.ts         # Standalone balance checker
│   ├── _gen_credential.ts       # Credential management
│   ├── generate_credentials.ts  # Credential generation utility
│   ├── allowance.ts             # Token allowance management
│   ├── bid_asker.ts             # Bid/ask price fetching
│   ├── market_order.ts          # Order execution
│   ├── market_finder.ts         # Market discovery
│   └── rpc-validator.ts         # RPC connection validation
├── .env                         # Environment variables (git-ignored)
├── .credentials.json            # Generated API credentials (git-ignored)
├── package.json                 # Dependencies and scripts
├── tsconfig.json                # TypeScript configuration
├── start-bot.ps1               # PowerShell startup script (Windows)
├── README.md                    # This file
├── QUICK_START.md              # Quick start guide
├── PROFIT_STRATEGY.md           # Detailed trading strategy guide
├── CREDENTIALS_GUIDE.md         # Credential generation guide
└── BALANCE_CHECK.md             # Balance checking documentation
```

### Core Modules

#### Credential Generator (`_gen_credential.ts`)

Manages wallet credentials and API authentication.

```typescript
import { CredentialGenerator } from './_gen_credential';

const generator = new CredentialGenerator();
generator.displayInfo();
```

#### Allowance Manager (`allowance.ts`)

Controls USDC token allowances for trading.

```typescript
import { AllowanceManager } from './allowance';

const manager = new AllowanceManager();
await manager.checkAllowance();
await manager.setAllowance('1000'); // Set 1000 USDC allowance
```

#### Bid/Ask Pricer (`bid_asker.ts`)

Retrieves real-time order book data.

```typescript
import { BidAsker } from './bid_asker';

const bidAsker = new BidAsker();
const data = await bidAsker.getPriceData(tokenId);
console.log(data.bidAsk.midpoint);
```

#### Market Order Executor (`market_order.ts`)

Places and manages orders.

```typescript
import { MarketOrderExecutor } from './market_order';

const executor = new MarketOrderExecutor();
await executor.placeMarketOrder({
    tokenId: 'TOKEN_ID',
    side: 'BUY',
    amount: 10 // 10 USDC
});
```

#### Market Finder (`market_finder.ts`)

Auto-detects and searches for markets.

```typescript
import { MarketFinder } from './market_finder';

const finder = new MarketFinder();
const market = await finder.findCurrentBitcoinMarket();
console.log(market.tokens); // UP and DOWN tokens
```

## Trading Strategy

### Overview

The automated bot implements a price arbitrage strategy that exploits price discrepancies between software price oracles and actual market prices.

### Strategy Flow

1. **Price Monitoring**: Continuously compares software oracle prices with Polymarket market prices
2. **Opportunity Detection**: Triggers trade when price difference exceeds configured threshold
3. **Three-Order Execution**:
   - Market Buy: Buys tokens at current market price
   - Take Profit Limit Sell: Sells when price rises to target
   - Stop Loss Limit Sell: Sells when price falls to limit
4. **Risk Management**: Configurable stop loss and take profit levels

### Example Trade Flow

```
Software Oracle calculates UP token worth: $0.75
Market selling UP token at: $0.70
Difference: $0.05 (above $0.015 threshold)

Bot executes:
1. BUY @ $0.70 (market order)
2. SELL @ $0.71 (take profit +$0.01)
3. SELL @ $0.695 (stop loss -$0.005)

Expected outcome:
- 70% chance: Take profit hits → +$0.01 profit
- 30% chance: Stop loss hits → -$0.005 loss
- Net expectation: Positive
```

For detailed strategy explanation, see [`PROFIT_STRATEGY.md`](./PROFIT_STRATEGY.md).

## Development

### Prerequisites

- TypeScript 5.9+
- Node.js 16+
- npm or yarn

### Development Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd polymarket-mirror-bot
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up environment**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

### Development Commands

```bash
# Development mode with auto-reload
npm run dev

# Type checking
npx tsc --noEmit

# Build for production
npm run build

# Run compiled version
npm start
```

### Windows Development

A PowerShell startup script is provided for Windows:

```powershell
# Run the automated bot
.\start-bot.ps1

# Or with execution policy bypass if needed
powershell -ExecutionPolicy Bypass -File start-bot.ps1
```

The script automatically loads environment variables from `.env` and starts the automated trading bot.

### Dependencies

**Core Dependencies:**
- `@polymarket/clob-client` - Official Polymarket CLOB client
- `ethers` - Ethereum wallet and cryptography
- `axios` - HTTP client for API requests
- `dotenv` - Environment variable management
- `ws` - WebSocket client for real-time data feeds

**Development Dependencies:**
- `typescript` - Type safety and modern JavaScript features
- `ts-node` - TypeScript execution environment
- `@types/node` - Node.js type definitions
- `@types/ws` - WebSocket type definitions

## Security

### Security Features

- ✅ Confirmation prompts before placing orders
- ✅ Price validation and sanity checks
- ✅ Automatic market price buffers
- ✅ Private key never exposed in logs
- ✅ Comprehensive error handling and recovery
- ✅ Automatic balance verification before trading
- ✅ Stop loss and take profit protection

### Security Best Practices

⚠️ **IMPORTANT SECURITY NOTES:**

- **Never commit your `.env` file** to version control
- **Keep your private key secure** and never share it
- **Test with small amounts first** before scaling
- **Review all transactions** before confirming
- The `.env` and `.credentials.json` files are automatically git-ignored
- Use a dedicated wallet for trading, not your main wallet
- Regularly rotate API credentials if compromised
- Monitor your wallet for unauthorized activity

### File Security

The following files contain sensitive information and are automatically git-ignored:
- `.env` - Contains private keys and API configuration
- `.credentials.json` - Contains API credentials

**Never share these files publicly.**

## Documentation

Additional documentation is available in the following files:

- [`QUICK_START.md`](./QUICK_START.md) - Quick start guide for new users
- [`PROFIT_STRATEGY.md`](./PROFIT_STRATEGY.md) - Detailed trading strategy explanation
- [`CREDENTIALS_GUIDE.md`](./CREDENTIALS_GUIDE.md) - Comprehensive credential generation guide
- [`BALANCE_CHECK.md`](./BALANCE_CHECK.md) - Balance checking feature documentation

### External Resources

- [Polymarket Documentation](https://docs.polymarket.com)
- [CLOB API Documentation](https://docs.polymarket.com/#clob-api)
- [Polygon Network Documentation](https://docs.polygon.technology/)

## License

ISC

## Disclaimer

**Use at your own risk.** This software is provided as-is without warranties of any kind. Trading involves financial risk and you may lose money. Always test with small amounts first. This software is for educational purposes only and is not financial advice.

### Important Notes

- This bot requires access to a software price oracle WebSocket server
- Ensure you have proper network access and permissions before running
- Past performance does not guarantee future results
- Trading involves risk of loss
- Consult with a financial advisor before making trading decisions
- This software is not affiliated with Polymarket

---

**Note**: This bot requires access to a software price oracle WebSocket server. Ensure you have proper network access and permissions before running.
