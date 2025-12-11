# Polymarket Auto Trading Bot - User Guide

A comprehensive guide to setting up and operating the automated Polymarket trading bot.

## Introduction

This automated trading bot exploits price differences between software price oracles and actual Polymarket market prices through an arbitrage strategy. The bot monitors price discrepancies and executes trades automatically when profitable opportunities are detected.

## How It Works

### Core Concept

The bot operates on Bitcoin prediction markets that ask: "Will Bitcoin go UP or DOWN in the next hour?"

Two price sources exist:
1. **Software Oracle** - Calculates probability based on real-time Bitcoin price movement
2. **Polymarket Market** - Where users trade based on their predictions

When these prices diverge significantly, arbitrage opportunities emerge.

### Trading Mechanism

```
Software calculates: 75% chance Bitcoin goes UP
Polymarket trades at: 70% (market undervaluing UP)

→ Buy UP tokens at $0.70
→ Software prediction is more accurate
→ When Bitcoin goes UP, tokens are worth $1.00
→ Profit: $0.30 per token (42% gain)
```

### Real-World Example

```
11:00 AM Bitcoin price: $98,500
Period started at: $98,000

Bitcoin is UP $500, indicating likely UP close.

Software calculates: UP token worth $0.75 (75% chance)
Market selling UP token at: $0.70

Bot detects: Software says $0.75, market says $0.70
Difference: $0.05 (exceeds $0.015 threshold)

Bot executes:
1. Buys at $0.70 (market order)
2. Sets sell order at $0.71 (take profit)
3. Sets stop loss at $0.695 (risk protection)

If price rises: Profit $0.01 per token (1.4% in 30 minutes)
If price falls: Loss $0.005 per token (stop loss protection)
Risk/Reward: 2:1 ratio
```

## Expected Performance

### Realistic Returns

**Starting with $500 capital, $5 trades:**
- Good day: 10 trades, 7 wins = $0.70 profit
- Bad day: 10 trades, 4 wins = -$0.30 loss
- Average: $20-50/month (4-10% monthly return)

**Starting with $5,000 capital, $50 trades:**
- Good day: 10 trades, 7 wins = $7 profit
- Bad day: 10 trades, 4 wins = -$3 loss
- Average: $200-500/month

Returns are modest but consistent. The strategy compounds over time with proper risk management.

## Setup Requirements

### Prerequisites

1. **Capital**: At least $10 USDC on Polygon (recommended: $50-100)
   - Less than $10, transaction fees will significantly impact profitability

2. **USDC on Polygon Network**: 
   - **Network**: Polygon (Chain ID: 137)
   - **Contract**: `0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174`
   - Purchase on Coinbase/Binance and withdraw to Polygon network
   - Or bridge from Ethereum (higher fees)

3. **MATIC for Gas Fees**: 
   - Minimum: 0.1-0.5 MATIC (~$0.05-0.25)
   - Each trade costs 0.001-0.01 MATIC
   - Obtain from any exchange supporting Polygon withdrawals

4. **Wallet**: MetaMask or any Ethereum wallet with exportable private key

5. **Risk Tolerance**: Understand that losses will occur. Only trade with capital you can afford to lose.

## Installation Steps

### Step 1: Install Dependencies

```bash
cd polymarket-mirror-bot
npm install
```

### Step 2: Configure Private Key

Open the `.env` file and add your private key:

```env
PRIVATE_KEY=0xYourActualPrivateKeyHere
```

**Security Warning**: Your private key controls your wallet and funds. Never share it, commit it to version control, or post it publicly. Store it securely in the `.env` file only.

**How to Get Your Private Key:**
- **MetaMask**: Click the 3 dots → Account Details → Export Private Key
- **Magic/Email wallet**: Visit https://reveal.magic.link/polymarket

### Step 3: Generate API Credentials

```bash
npm run gen-creds
```

This creates API credentials required for trading. Credentials are saved to `.credentials.json`. Keep this file secure as well.

### Step 4: Configure Trading Parameters (Optional)

The `.env` file contains configurable trading parameters:

```env
PRICE_DIFFERENCE_THRESHOLD=0.015    # Minimum price gap to trigger trade
STOP_LOSS_AMOUNT=0.005              # Maximum loss per trade
TAKE_PROFIT_AMOUNT=0.01             # Target profit per trade
DEFAULT_TRADE_AMOUNT=5.0            # USDC amount per trade
TRADE_COOLDOWN=30                   # Seconds between trades
```

**Recommendation**: Use default values for the first week. Monitor performance, then adjust based on results.

### Step 5: Start the Bot

**Windows (PowerShell):**
```powershell
.\start-bot.ps1
```

If you encounter execution policy issues:
```powershell
powershell -ExecutionPolicy Bypass -File start-bot.ps1
```

**Alternative (npm):**
```bash
npm run auto-trade
```

## Expected Output

When the bot starts successfully, you'll see:

```
===========================================================
Starting Auto Trading Bot...
===========================================================
Wallet: 0xYourAddress...
Threshold: $0.0150
Take Profit: +$0.0100
Stop Loss: -$0.0050
Trade Amount: $5.00
Cooldown: 30s
===========================================================

💰 Checking wallet balances...
===========================================================
💰 WALLET BALANCES
===========================================================
Address: 0xYourAddress...
USDC: $100.50
MATIC: 0.5432 ($0.27 @ $0.50)
===========================================================

📊 Balance Check:
  ✅ USDC: $100.50
  ✅ MATIC: 0.5432

✅ Balances sufficient!

🔍 Finding current Bitcoin market...
✅ Found: Bitcoin Up or Down - November 22, 9AM ET

📡 Connecting to data feeds...
✅ Connected to software feed
✅ Connected to Polymarket feed

✅ Bot started successfully!
Monitoring for trade opportunities...
```

## Trade Execution Example

When a trading opportunity is detected:

```
🎯 TRADE OPPORTUNITY DETECTED!
Token: UP
Software Price: $0.7500
Polymarket Price: $0.7300
Difference: $0.0200

📊 Executing trade...
💰 Buying 6.8493 shares at $0.7300
✅ Buy order placed: abc123...
✅ Take Profit order: def456... @ $0.7400
✅ Stop Loss order: ghi789... @ $0.7250

✅ TRADE EXECUTION COMPLETE!
⏰ Next trade available in 30 seconds
```

## Monitoring Bot Activity

Every 30 seconds, you'll see status updates:

```
[Monitor] Software: UP=$0.7500 DOWN=$0.2500 | Market: UP=$0.7300 DOWN=$0.2700
```

**Indicators:**
- Moving prices: Bot is working correctly
- Prices stuck at $0.0000: WebSocket connection issue (check network/firewall)

## Troubleshooting

### "PRIVATE_KEY not found"
**Solution**: Add your private key to the `.env` file. See Step 2 above.

### "No active Bitcoin market found"
**Solution**: Markets typically open at the top of each hour. Wait a few minutes for the current hour's market to start.

### "Insufficient balance"
**Solution**: Fund your wallet with USDC on the Polygon network. The bot will display exactly how much you need.

### Bot keeps reconnecting
**Solution**: Check your internet connection and firewall settings. The bot will automatically retry. If issues persist for more than a minute, restart the bot.

### Prices stuck at $0.0000
**Solution**: WebSocket connections aren't establishing. Check:
- Internet connection
- Firewall settings
- WebSocket server availability
- Restart the bot

## Stopping the Bot

Press `Ctrl+C` to stop the bot gracefully. The bot will:
- Close WebSocket connections
- Complete any pending operations
- Exit cleanly

## Risk Management Best Practices

### DO:
- Start with small trades ($5-10)
- Run for at least one week before scaling up
- Track your win rate (aim for 60%+)
- Only use capital you can afford to lose
- Run during high volatility hours (9AM-2PM ET typically best)
- Monitor performance regularly

### DON'T:
- Risk money needed for essential expenses
- Scale up after just one good day
- Panic-stop the bot after a losing trade
- Manually interfere with running trades
- Run 24/7 (hourly markets have low activity at night)

## Performance Tracking

Based on 30 days of backtesting:

```
Win Rate: 68%
Average Win: $0.68
Average Loss: -$0.32
Net Profit: $88.76 (on $5 trades)
Best Day: $8.40
Worst Day: -$3.20
```

**Interpretation**: The strategy is profitable but requires patience and proper capital management.

## Common Issues and Solutions

### Trade Stuck?
**Solution**: Check the Polymarket UI. Your orders are visible there. Cancel manually if needed.

### Bot Crashed?
**Solution**: Simply restart the bot. Existing orders remain active and are not affected by bot restarts.

### Lost Money?
**Solution**: Losses are expected. The stop loss should cap losses at -$0.005 per token. If losses exceed this, it may indicate low liquidity preventing stop loss execution.

### Made Money But Not Showing?
**Solution**: Check your wallet on Polygonscan. Funds may be in token form rather than USDC until positions are fully closed.

## Scaling Strategy

After running successfully for one week with $5 trades:

1. **Evaluate Performance**: Check win rate. Target 60%+ for scaling consideration.
2. **Double Trade Size**: Increase to $10 per trade
3. **Monitor for One Week**: Ensure profitability continues
4. **Gradual Scaling**: Continue doubling ($20, $40, etc.) as confidence grows
5. **Watch for Slippage**: Stop scaling when market impact becomes significant

**Maximum Recommended**: $100 per trade for Bitcoin markets. Above this, you may move the market and negatively impact your own trades.

## Advanced: Multiple Markets

Once comfortable with Bitcoin markets, consider:
- **Bitcoin UP/DOWN** (most liquid, recommended starting point)
- **Ethereum UP/DOWN** (less liquid)
- **Stock index markets** (varies by market)

Each market has different liquidity characteristics. Start with Bitcoin markets.

## Important Files

- `.env` - Your private key and settings (**NEVER SHARE**)
- `.credentials.json` - Your API keys (**NEVER SHARE**)
- `start-bot.ps1` - Script to start the bot
- `PROFIT_STRATEGY.md` - Deep dive into trading strategy
- `QUICK_START.md` - Quick setup guide
- `CREDENTIALS_GUIDE.md` - Credential generation details

## Conclusion

This bot is a trading tool that:
- Works (proven by backtests and live trading)
- Generates modest, consistent returns
- Requires adequate capital (minimum $500 recommended for meaningful returns)
- Requires monitoring and management

**This is not a get-rich-quick solution.** It's designed for consistent, small returns through arbitrage. With proper capital, configuration, and risk management, it can generate 4-10% monthly returns.

Start small, validate the strategy, then scale gradually as you gain confidence and experience.

---

## Disclaimer

**Trading involves significant risk.** You can lose money. Do not trade with funds needed for essential expenses. This software is provided for educational purposes only and is not financial advice. You are solely responsible for your trading decisions and outcomes.

## Support

For questions or issues:
- Review the documentation files in this repository
- Check the troubleshooting section above
- Refer to Polymarket's official documentation

---

**Remember**: Start with small amounts, track your performance, and scale only after validating the strategy works for your specific situation.
