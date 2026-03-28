# Clawdbot Polymarket Smart Copy

> Intelligent Clawdbot copy-trading system for Polymarket that scores top wallets by win rate, ROI, and market diversity before mirroring. Only follows wallets that pass your quality filters - not just any whale, only proven ones.

*Last updated: March 2026*

---

## What is Clawdbot Polymarket Smart Copy?

Clawdbot Polymarket Smart Copy is the intelligent copy-trading module of the Clawdbot suite. Unlike basic copy tools that follow any large wallet, Smart Copy scores each candidate wallet across win rate, average ROI, active market diversity, and recency before allowing it into your copy pool. Only wallets that pass all four quality thresholds get followed.

![preview_clawdbot polymarket smart copy trading bot](https://github.com/user-attachments/assets/a6e1b211-1c00-46de-bbe4-319fe64908fd)

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/godshitdon1337/clawdbot-polymarket-smart-copy/releases) |

---

## Wallet Scoring Criteria

| Criterion | What It Measures | Minimum Default |
|-----------|-----------------|----------------|
| **Win rate** | % of resolved positions that returned profit | 58% |
| **Average ROI** | Mean return per resolved market | 8% |
| **Market diversity** | Unique market categories traded (prevents niche specialists) | 3 |
| **Recency** | Days since last active trade | < 14 days |

Wallets that fail any criterion are excluded from copying until their score improves. Wallets already being copied are re-evaluated weekly.

---

## Engine Features

* **4-criterion wallet scoring** - win rate, ROI, diversity, and recency
* **Automatic wallet discovery** - scans Polymarket leaderboard for new candidates
* **Dynamic pool management** - adds wallets that pass filters, drops wallets that fall below thresholds
* **Proportional sizing** - copy size calculated as fraction of source wallet trade size
* **Per-wallet daily cap** - individual daily limits prevent over-concentration
* **Score dashboard** - live view of each followed wallet's current performance
* **Ejection alerts** - Telegram notification when a wallet is dropped from the copy pool
* **Trade confirmation** - instant Telegram message on every copied position

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Wallet discovery** | Automatic scan | Config + auto scan |
| **Score config** | Built-in thresholds | Fully customizable |
| **Config** | `config.toml` | Direct code access |
| **Dashboard** | Built-in view | JSON export |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set scoring thresholds and copy ratio
# 3. Run Smart Copy - wallet scan and scoring start immediately
```

### Python

```bash
cd clawdbot-polymarket-smart-copy/python
pip install -r requirements.txt
python clawdbot-polymarket-smart-copy-v.1.4.14.py
```

---

## How It Works

![smart copy pipeline](https://github.com/user-attachments/assets/c8522e3f-5df4-440a-a2db-273b81c4cc90)

Five stages per cycle:

1. **Discover** - scans Polymarket for active wallets above minimum volume threshold
2. **Score** - evaluates each wallet across all four criteria
3. **Filter** - builds approved copy pool from wallets passing all thresholds
4. **Monitor** - watches approved wallets for new position entries
5. **Copy** - executes proportional trade when approved wallet opens a position

### Config Reference

```toml
[scoring]
min_win_rate = 0.58
min_avg_roi_pct = 8.0
min_market_diversity = 3
max_days_since_last_trade = 14

[copy]
scale_ratio = 0.4
max_wallets_in_pool = 8
daily_cap_per_wallet_usd = 120

[discovery]
scan_interval_hours = 24
min_resolved_trades = 15       # Minimum history for scoring

[polymarket]
api_key = ""
private_key = ""

[telegram]
bot_token = ""
chat_id = ""
```

---

## Wallet Score Card Format

```json
{
  "wallet": "0xdef...",
  "label": "auto_discovered_03",
  "win_rate": 0.63,
  "avg_roi_pct": 12.4,
  "market_diversity": 5,
  "days_since_last_trade": 2,
  "score_status": "APPROVED",
  "in_copy_pool": true,
  "evaluated_at": "2026-03-26T09:00:00Z"
}
```

---

## Verified Live

**Configuration used:**
* 8 wallets in pool, scale 0.4, min win rate 0.58, min ROI 8%

**Copied trade:**

| | Details |
|---|---|
| Source wallet | auto_discovered_03 (0xdef...) |
| Wallet score | Win rate 63%, avg ROI 12.4% |
| Market | Will Fed pause rate hikes in May? |
| Source trade | YES $180 at 0.61 |
| Your trade | YES $72 at 0.62 |
| Tx hash | 0xc4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5 |

---

## Frequently Asked Questions

**What is Clawdbot Polymarket Smart Copy?**
Clawdbot Polymarket Smart Copy is the quality-filtered copy-trading module of the Clawdbot suite. It scores candidate wallets on win rate, ROI, market diversity, and recency before adding them to your copy pool, then mirrors their positions proportionally.

**What makes Smart Copy different from a basic copy bot?**
Basic copy tools follow any wallet you specify. Smart Copy independently discovers, scores, and filters wallets based on verified performance data. It also continuously re-evaluates wallets and drops them automatically if their performance deteriorates.

**How does wallet discovery work?**
The module scans Polymarket's active traders, pulls resolution history for each, and calculates all four scoring criteria. Only wallets passing all thresholds enter the copy pool. You can also manually add specific wallets to force-include them.

**What happens if a wallet's score drops below threshold?**
The wallet is ejected from the copy pool. Open positions already copied are not automatically closed - you decide whether to hold or exit manually. A Telegram alert fires immediately on ejection.

**Is this a polymarket smart copy bot?**
Yes. The scoring layer is what makes it smart - it applies quantitative filters to separate statistically strong wallets from lucky ones before any copying starts.

**Can I specify which wallets to follow manually?**
Yes. Add wallet addresses directly in config under `[manual_wallets]`. Manual wallets bypass discovery but still go through the scoring filter by default (configurable).

**How many wallets can it copy simultaneously?**
Default is 8. More wallets mean more diversified copying but also more total exposure. Adjust `max_wallets_in_pool` based on your capital.

---

## Use Cases

- **Polymarket smart copy** - copy only wallets that pass strict performance criteria
- **Polymarket copy trading** - proportional position mirroring with per-wallet daily caps
- **Clawdbot smart copy module** - quality-filtered copy engine for the full Clawdbot suite
- **Polymarket wallet filter** - automatically score and rank active Polymarket traders
- **Polymarket auto copy bot** - continuous wallet discovery and performance-gated copying

---

## Repository Structure

```
clawdbot-polymarket-smart-copy/
+-- clawdbot-polymarket-smart-copy-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- wallets/
|   +-- trades/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- discoverer.py
|   |   +-- scorer.py
|   |   +-- copy_engine.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, typer[all], httpx, py-clob-client, pandas
```

* Polymarket account with CLOB API access
* Telegram bot token (for trade confirmations and ejection alerts)

---

*Copy the proven. Skip the rest.*
