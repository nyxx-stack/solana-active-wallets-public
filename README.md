# 🚀 Solana Active Wallets Data

This repository contains curated lists of unique active Solana wallet addresses that meet specific criteria for trading activity.

## 📊 What Qualifies as an "Active Wallet"?

Active wallets are Solana addresses that have conducted transactions within the last 24 hours and meet ALL of the following criteria:

### Trading Activity Filters:
- **Trade Volume**: Between $500 and $50,000 USD in 24-hour trading volume
- **Token Type**: Traded meme coins and other tokens (excludes SOL, USDC, USDT stablecoins)
- **Bot Exclusion**: Fewer than 100 trades in 24 hours (to filter out high-frequency trading bots)

### Additional Filtering:
- **Frequency Filter**: Addresses that appear 3 or more times across recent daily batches are excluded. This helps identify and remove addresses that repeatedly show up day after day, likely indicating automated trading systems or market makers rather than genuine retail traders.
- **Time Window**: Rolling 24-48 hour window for latest data

## 📁 Repository Structure

```
├── latest.json                      # Most recent wallet addresses (rolling 24-48h window)
└── history/                         # Historical daily snapshots
    └── YYYY-MM-DD.json             # Frequency-filtered daily addresses
```

## 🔄 Update Schedule

Data is updated every 6 hours at the following UTC times:
- **00:00 UTC** (4PM PST previous day)
- **06:00 UTC** (10PM PST previous day)
- **12:00 UTC** (4AM PST)
- **18:00 UTC** (10AM PST)

Daily consolidated snapshots are created after the 18:00 UTC run.

## 📊 Data Files

### Primary Files:
- **`latest.json`**: Most recent wallet addresses (rolling 24-48h window) - always current
- **`history/YYYY-MM-DD.json`**: Daily snapshots with frequency filtering applied

### Data Format:
Each JSON file contains an array of unique wallet addresses that met all filtering criteria during the specified time period.

## 🎯 Use Cases

This data is suitable for:
- Identifying active retail traders on Solana
- Analyzing meme coin trading patterns
- Building targeted wallet lists for analysis
- Excluding bot activity from trading analysis

## 📝 Notes

- Only includes public on-chain data
- Automated updates ensure fresh data every 6 hours
- Historical data retained for 14 days (older files auto-deleted)
- Frequency filtering helps identify unique traders vs. repeat actors