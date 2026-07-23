# XRPL Trading Bot v2.0

A modular, high-performance XRPL trading bot with sniper and copy trading capabilities. This version has been completely refactored from the original Telegram bot into a standalone, modular architecture.

## 🚀 Features

- **Token Sniping**: Automatically detect and snipe new tokens from AMM pools
- **Copy Trading**: Mirror trades from successful wallets in real-time
- **Modular Architecture**: Clean, maintainable codebase split into logical modules
- **High Performance**: Optimized for speed and efficiency
- **Configurable**: Easy-to-use configuration system

## 📁 Project Structure

```
xrpl-trading-bot/
├── src/
│   ├── config/          # Configuration management
│   ├── database/         # Database models and connection
│   ├── xrpl/             # XRPL client, wallet, and AMM utilities
│   ├── sniper/           # Token sniping module
│   ├── copyTrading/      # Copy trading module
│   ├── types/            # TypeScript type definitions
│   └── bot.ts            # Main bot orchestrator
├── dist/                 # Compiled JavaScript (after build)
├── index.ts              # Entry point (TypeScript)
├── filterAmmCreate.ts    # AMM transaction checker utility
├── tsconfig.json          # TypeScript configuration
├── package.json
└── .env                  # Environment configuration
```

**Note**: This project is written in TypeScript and compiles to JavaScript in the `dist/` folder.

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd xrpl-trading-bot
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Build TypeScript** (required before running)
   ```bash
   npm run build
   ```

4. **Configure environment variables**
   Create a `.env` file:
   ```env
   # XRPL Configuration
   XRPL_SERVER=wss://xrplcluster.com
   XRPL_NETWORK=mainnet

   # Wallet Configuration (REQUIRED)
   WALLET_SEED=your_wallet_seed_here
   WALLET_ADDRESS=your_wallet_address_here

   # Storage Configuration (Optional)
   DATA_FILE=./data/state.json

   # Trading Configuration (Optional)
   MIN_LIQUIDITY=100
   MAX_SNIPE_AMOUNT=5000
   DEFAULT_SLIPPAGE=4.0
   SNIPER_CHECK_INTERVAL=8000
   COPY_TRADING_CHECK_INTERVAL=3000

   # Sniper Configuration (Optional)
   SNIPER_BUY_MODE=true
   SNIPER_AMOUNT=1
   SNIPER_CUSTOM_AMOUNT=
   SNIPER_MIN_LIQUIDITY=100
   SNIPER_RISK_SCORE=medium
   SNIPER_TRANSACTION_DIVIDES=1

   # Copy Trading Configuration (Optional)
   COPY_TRADER_ADDRESSES=rTrader1Address,rTrader2Address
   COPY_TRADING_AMOUNT_MODE=percentage
   COPY_TRADING_MATCH_PERCENTAGE=50
   COPY_TRADING_MAX_SPEND=100
   COPY_TRADING_FIXED_AMOUNT=10
   ```

## 🎯 Usage

### Quick Start (No Build Required)
After `npm install`, you can run the bot directly:

```bash
# Start both sniper and copy trading
npm start

# Start only sniper
npm run start:sniper

# Start only copy trading
npm run start:copy

# Start with custom user ID
npm start -- --user=my-user-id
```

### Development (with auto-reload)
```bash
npm run dev:watch
```

### Production (Optional: Compiled JavaScript)
If you prefer to compile to JavaScript first:
```bash
# Build first
npm run build

# Then run compiled version
node dist/index.js
```

**Note:** The bot runs directly from TypeScript source using `ts-node`, so no build step is required. The `build` script is optional for users who prefer compiled JavaScript.

## 📋 Prerequisites

Before running the bot, you need to:

1. **Configure Wallet**: Set `WALLET_SEED` and `WALLET_ADDRESS` in `.env`
2. **Fund Wallet**: Ensure your wallet has sufficient XRP for trading and fees
3. **Configuration**: All configuration is done via `.env` file. The `data/state.json` file is only used for runtime state (transactions, purchases, balances).

## ⚙️ Configuration

### Configuration via .env

All configuration is done through the `.env` file. The `data/state.json` file is automatically created and only stores runtime state (transactions, purchases, balances).

### Sniper Configuration

Configure sniper settings in `.env`:

- `SNIPER_BUY_MODE`: `true` for auto-buy mode, `false` for whitelist-only
- `SNIPER_AMOUNT`: Amount to snipe (e.g., '1', '5', '10', 'custom')
- `SNIPER_CUSTOM_AMOUNT`: Custom snipe amount if using 'custom' mode
- `SNIPER_MIN_LIQUIDITY`: Minimum liquidity required (XRP)
- `SNIPER_RISK_SCORE`: Risk tolerance ('low', 'medium', 'high')
- `SNIPER_TRANSACTION_DIVIDES`: Number of transactions to divide snipe into

**Note:** Whitelist and blacklist tokens are still configured in `data/state.json` as they are runtime data.

### Copy Trading Configuration

Configure copy trading settings in `.env`:

**Required Settings:**
- `COPY_TRADER_ADDRESSES`: Comma-separated list of trader wallet addresses to copy
  ```env
  COPY_TRADER_ADDRESSES=rTrader1Address,rTrader2Address
  ```

**Optional Settings:**
- `COPY_TRADING_AMOUNT_MODE`: Trading amount calculation mode (`percentage`, `fixed`, or `match`)
- `COPY_TRADING_MATCH_PERCENTAGE`: Percentage of trader's amount to match (0-100)
- `COPY_TRADING_MAX_SPEND`: Maximum XRP to spend per copy trade
- `COPY_TRADING_FIXED_AMOUNT`: Fixed XRP amount for each copy trade

**Note:** All configuration is now in `.env`. The `data/state.json` file only stores runtime state (transactions, purchases, balances, active flags).

## 🔧 Module Overview

### Sniper Module (`src/sniper/`)
- **monitor.ts**: Detects new tokens from AMM create transactions
- **evaluator.ts**: Evaluates tokens based on user criteria (rugcheck, whitelist, etc.)
- **index.ts**: Main sniper logic and orchestration

### Copy Trading Module (`src/copyTrading/`)
- **monitor.ts**: Monitors trader wallets for new transactions
- **executor.ts**: Executes copy trades based on detected transactions
- **index.ts**: Main copy trading logic and orchestration

### XRPL Module (`src/xrpl/`)
- **client.ts**: XRPL WebSocket client management
- **wallet.ts**: Wallet operations and utilities
- **amm.ts**: AMM trading functions (buy/sell)
- **utils.ts**: XRPL utility functions

## 🛡️ Safety Features

- Maximum snipe amount limits
- Minimum liquidity requirements (rugcheck)
- Blacklist/whitelist filtering
- Slippage protection
- Transaction deduplication
- Balance validation before trades

## 📊 Monitoring

The bot logs all activities to the console:
- ✅ Successful operations
- ⚠️ Warnings
- ❌ Errors
- 🎯 Sniper activities
- 📊 Copy trading activities

## ⚠️ Important Notes

- **Mainnet Only**: This bot operates on XRPL mainnet with real funds
- **Risk Warning**: Trading cryptocurrencies involves substantial risk
- **No Guarantees**: Past performance doesn't guarantee future results
- **Test First**: Always test with small amounts first

## 🔄 Migration from v1.0

If you're migrating from the Telegram bot version:

Key changes:
- Removed all Telegram dependencies
- Removed MongoDB dependency (now uses JSON file storage)
- Modular architecture (was 9900+ lines in one file)
- Runs as standalone process instead of Telegram bot
- State is stored in `data/state.json` instead of MongoDB

**Note**: If you have existing MongoDB data, you'll need to export it and convert to the JSON format. See `data/state.json.example` for the structure.

## 📝 License

MIT License - Use at your own risk.

## 🤝 Contributing

Contributions are welcome! Please ensure your code follows the existing modular structure.

---

**⚠️ Disclaimer**: This bot is for educational purposes. Use at your own risk. The developers are not responsible for any financial losses.

## 📞 Contact

For support or questions, reach out on 
Telegram: [@cryptomoonday](https://t.me/cryptomooonday23)
Twitter: https://x.com/cryptomoonday
Discord: @cryptomoonday

**⭐ Star**: this repository if you find it useful!
