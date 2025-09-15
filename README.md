# Solana Active Wallets Public

This repository contains curated lists of active Solana wallet addresses that trade meme coins. Updated every 6 hours via automated system.

## Repository Structure

```
solana-active-wallets-public/
├── latest/                          # Always current (rolling 24-48h window)
│   ├── YYYY-MM-DD/                  # Today's data
│   │   ├── run-00.json              # 00:00-06:00 UTC slot
│   │   ├── run-06.json              # 06:00-12:00 UTC slot
│   │   ├── run-12.json              # 12:00-18:00 UTC slot
│   │   └── run-18.json              # 18:00-00:00 UTC slot
│   └── last_updated.json            # Update metadata
├── history/                         # Daily snapshots (last 14 days)
│   └── YYYY-MM-DD.json              # Frequency-filtered daily addresses
└── weekly-archives/                 # Weekly consolidated archives
    └── YYYY-Www.json                # ISO week format (e.g., 2024-W52.json)
```

## Data Files

### For Real-Time Analysis
- **`latest/YYYY-MM-DD/*.json`** - Current day's addresses from each 6-hour run
- Updated every 6 hours at 00:00, 06:00, 12:00, 18:00 UTC

### For Daily Batch Analysis
- **`history/YYYY-MM-DD.json`** - Consolidated daily snapshots with frequency filtering
- Created after 18:00 UTC run completes
- Contains only "fresh" wallet addresses (not seen frequently before)

### Weekly Archives
- **`weekly-archives/YYYY-Www.json`** - Historical data consolidated by week
- Daily files older than 14 days are archived here
- Used for enhanced frequency filtering

## Frequency Filtering

The system uses **cumulative frequency filtering** to ensure only fresh wallets appear in daily batches:

1. **Counts appearances across:**
   - Recent daily files (last 14 days)
   - All weekly archives (historical data)

2. **Filter threshold:** Addresses appearing 3+ times total (cumulative) are excluded

3. **Examples:**
   - 2 times in daily + 1 time in weekly = 3 total → FILTERED
   - 1 time in daily + 2 times in weekly = 3 total → FILTERED
   - 2 times in daily + 0 times in weekly = 2 total → KEPT

## Update Schedule

| Run Slot | UTC Time | PST/PDT Time | Description |
|----------|----------|--------------|-------------|
| run-00 | 00:00-06:00 | 16:00-22:00 (prev day) | Evening activity |
| run-06 | 06:00-12:00 | 22:00-04:00 | Night activity |
| run-12 | 12:00-18:00 | 04:00-10:00 | Morning activity |
| run-18 | 18:00-00:00 | 10:00-16:00 | Day activity |

Daily consolidation happens after run-18 completes (~18:00 UTC).

## Wallet Criteria

Addresses in this repository represent wallets that:
- Trade meme coins (non-SOL, non-stablecoin tokens)
- Have trading volume between $500-$50,000
- Execute fewer than 100 trades (bot filter)
- Maintain >5 SOL balance
- Haven't appeared frequently in historical data

## Usage

### Python
```python
import json
import requests

# Get today's fresh wallets (for batch analysis)
response = requests.get(
    "https://raw.githubusercontent.com/nyxx-stack/solana-active-wallets-public/main/history/2025-01-15.json"
)
addresses = response.json()

# Get latest addresses (real-time)
response = requests.get(
    "https://raw.githubusercontent.com/nyxx-stack/solana-active-wallets-public/main/latest/2025-01-15/run-18.json"
)
latest = response.json()
```

### Direct Download
```bash
# Today's consolidated addresses
curl -O https://raw.githubusercontent.com/nyxx-stack/solana-active-wallets-public/main/history/$(date +%Y-%m-%d).json

# Latest run
curl -O https://raw.githubusercontent.com/nyxx-stack/solana-active-wallets-public/main/latest/$(date +%Y-%m-%d)/run-18.json
```

## Data Retention

- **Daily files:** Last 14 days kept as individual files
- **Weekly archives:** Older data consolidated by ISO week (YYYY-Www format)
- **No data loss:** All historical addresses preserved in weekly archives

## Notes

- All timestamps are in UTC
- Addresses are Solana wallet public keys (base58 format)
- Data sourced from Bitquery APIs
- Automated updates via Google Cloud Functions

## License

This data is provided as-is for research and analysis purposes.

---

*Automated updates every 6 hours via [sol-wallet-query](https://github.com/nyxx-stack/sol-wallet-query)*