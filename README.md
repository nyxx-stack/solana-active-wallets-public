# Solana Active Wallets Public

## Overview
This repository provides curated lists of Solana wallet addresses engaged in meme coin trading. The data updates automatically every 6 hours.

## Repository Structure

The repository organizes data into three main directories:

- **latest/**: Contains current trading data with rolling 24-48 hour windows, divided into six-hour increments (run-00, run-06, run-12, run-18)
- **history/**: Maintains daily snapshots from the past 14 days with frequency filtering applied
- **weekly-archives/**: Archives older data using ISO week format (YYYY-Www pattern)

## Data Access

**For Real-Time Analysis**: Access `latest/YYYY-MM-DD/*.json` files updated at 00:00, 06:00, 12:00, and 18:00 UTC.

**For Batch Processing**: Use `history/YYYY-MM-DD.json` files created after the 18:00 UTC run completes, containing only "fresh" wallet addresses.

## Frequency Filtering Mechanism

The system implements cumulative frequency filtering across recent daily files (14 days) and all historical weekly archives. Wallets appearing 3+ times cumulatively are filtered out.

Example: "2 times in daily + 1 time in weekly = 3 total → FILTERED"

## Wallet Selection Criteria

**Trading Requirements**:
- Trade non-SOL, non-stablecoin meme coins
- Volume between $100-$50,000 per time window
- Fewer than 100 trades (bot detection)
- Average trade size between $50-$2,500

**Quality Standards**:
- Minimum 1 trade and 1 distinct transaction
- Minimum 1 unique token traded

## Usage Examples

**Python**: Use `requests` library to fetch from raw GitHub URLs for both historical and real-time data.

```python
import requests

# Fetch latest run
url = "https://raw.githubusercontent.com/nyxx-stack/solana-active-wallets-public/main/latest/YYYY-MM-DD/run-00.json"
response = requests.get(url)
wallets = response.json()
```

**Command Line**: Download files via `curl` with dynamic date formatting.

```bash
# Fetch today's 00:00 UTC run
curl -O "https://raw.githubusercontent.com/nyxx-stack/solana-active-wallets-public/main/latest/$(date -u +%Y-%m-%d)/run-00.json"
```

## Data Format

Each JSON file contains an array of wallet objects with the following structure:

```json
[
  {
    "address": "wallet_address_in_base58",
    "usd_volume": 1234.56,
    "trade_count": 15,
    "unique_tokens": 3,
    "distinct_transactions": 12,
    "avg_trade_usd": 82.30
  }
]
```

## Additional Information

- Data sourced from blockchain analytics APIs
- All timestamps in UTC
- Addresses in base58 format (Solana public keys)
- Historical data fully preserved in weekly archives
- Known bot and spam addresses are excluded from all datasets

## Update Schedule

Data refreshes occur at:
- 00:00 UTC
- 06:00 UTC
- 12:00 UTC
- 18:00 UTC

Daily history snapshots are created after the 18:00 UTC run completes.

## Notes

- This is a data-only repository; the processing system is maintained separately
- Frequency filtering helps identify genuinely active traders rather than automated bots
- Volume and quality thresholds are tuned to capture organic meme coin trading activity
