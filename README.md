# 💎 Polymarket Streak-Reversal Martingale Bot v2.2

[![Status](https://img.shields.io/badge/Status-Production--Ready-brightgreen)](https://github.com/satyamsk05/2in)
[![Version](https://img.shields.io/badge/Version-2.2.0-blue)](https://github.com/satyamsk05/2in)
[![Strategy](https://img.shields.io/badge/Strategy-Martingale--Reversal-orange)](https://github.com/satyamsk05/2in)
[![Interval](https://img.shields.io/badge/Interval-15--Minute-blueviolet)](https://github.com/satyamsk05/2in)

A professional-grade, multi-market trading suite for Polymarket. This bot specializes in **Streak-Reversal Martingale** strategies on **15-minute price markets**, engineered for 24/7 autonomous stability and high-fidelity monitoring.

---

## 🚀 Key Features

### 🛡️ Production Hardening (New in v2.2)
Specifically engineered to resolve common bot crashes and race conditions:
- **API Rate Limiting**: Built-in `Semaphore(2)` prevents `429 Too Many Requests` during high-concurrency market boundaries.
- **Thread Safety**: `CandleStore` uses atomic `threading.Lock()` to ensure data integrity when processing BTC, ETH, SOL, and XRP simultaneously.
- **PID Guard + Process Verification**: Prevents multiple instances with robust Linux process checks (via `/proc`), identifying stale PID files automatically.
- **Recovery Priority**: Smart logic prioritizes trades with the highest Martingale step, focusing on recovering losses first.

### 🍱 High-Fidelity Terminal Dashboard
A premium, real-time terminal interface built with `Rich`:
- **Live Orderbook**: Instant monitoring of `up_ask` and `down_ask` prices.
- **MG Ladder Visualization**: Active tracking of Martingale steps (◉○○○○) and bet amounts per coin.
- **Unified Logging**: Fixed internal panel showing real-time system events without terminal flicker.

### 📱 Interactive Telegram Hub
Full-featured 3x3 mobile command center:
- **🖥 Live State**: View current prices and seconds remaining in the market.
- **🏦 Wallet**: Check Virtual (Dry Run), Real (On-chain), and Locked (In-bets) balances.
- **⚡ Quick Bet**: Interactive guide to place manual $N trades on the fly.
- **📊 PnL Analytics**: Daily and 7-day profit/loss reporting with detailed fee tracking.

---

## 🏗️ Project Structure

```text
2in/
├── src/
│   ├── main.py                # Core execution loop & Parallel processor
│   ├── data_feed.py           # Multi-market WebSocket client
│   ├── strategy.py            # Martingale reversal & Thread-safe CandleStore
│   ├── telegram_bot.py        # Interactive 3x3 menu UI
│   ├── history_manager.py     # Persistence, PnL & Bet tracking
│   └── utils/
│       ├── gsd_logger.py      # Centralized thread-safe logging
│       └── metrics_manager.py # JSON health & stats exporter
├── run.py                     # Watchdog Supervisor (Production ENTRY POINT)
├── history/                   # Persistent trade & PnL logs
├── data/                      # Live metrics & Virtual balance
└── .env                       # Secret keys & Configuration
```

---

## ⚙️ Setup & Installation

### 1. Requirements
- Python 3.9+
- USDC.e or Native USDC on Polygon network.
- Polymarket CLOB API Credentials.

### 2. Fast Installation
```bash
git clone https://github.com/satyamsk05/2in.git
cd 2in

# Environment Setup
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Configure Secrets
cp .env.example .env
nano .env # Fill in keys
```

### 3. Running in Production
Always use `run.py`. It includes a **Watchdog Supervisor** that auto-restarts the bot if it crashes due to network issues or OOM.
```bash
python3 run.py
```

---

## 📊 Strategy: Martingale Streak-Reversal

The bot monitors 15-minute price market boundaries:
1. **Detection**: If the last **3 closes** are in the same direction (e.g., 🔴🔴🔴), the bot signals a **Reversal** bet for the next candle.
2. **Execution**:
   - **Step 0**: Uses `FOK` (Fill-or-Kill) orders for instant entry.
   - **Step 1+**: Uses `GTC` (Good-Till-Cancelled) orders with specific price brackets to manage recovery risk.
3. **Martingale Ladder**:
   - Default: `$3 → $6 → $13 → $28 → $60`
   - Resets to `$3` immediately after any **WIN**.

---

## 📱 Telegram Command Reference

| Command | Description |
|:--- |:--- |
| `🖥 Live` | View current market prices and timers |
| `🏦 Wallet` | Show Virtual, Real, and Locked USDC balances |
| `📦 Open` | List all currently active positions |
| `📜 Log` | Show last 10 completed trades with results |
| `📈 Trend` | Visual streak analysis (Circles: 🟢🔴⚪) |
| `📊 PnL` | Detailed Profit/Loss summary for the last 7 days |
| `🩺 System` | Health check: Uptime, WS status, and log size |
| `⚡ Quick Bet` | Start an interactive manual trade flow |
| `⏹ Pause/▶ Resume` | Toggle the bot's trading capability |

---

## 💡 Troubleshooting
- **API 429 Errors**: The bot now includes auto-semaphores to prevent this.
- **Stale PIDs**: If the bot refuses to start, check `data/bot.pid`. v2.2 now handles this automatically.
- **No Signal**: Bot needs 3 complete 15m candles to form a reversal signal.

---
© 2026 Polymarket Pro Bot Team. | **Hardened for 24/7 Autonomous Stability.**
