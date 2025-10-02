# Binance Futures Order Bot (CLI)

This project is a CLI-based trading helper for Binance USDT-M Futures. It supports:

- Market orders (mandatory)
- Limit orders (mandatory)
- Advanced: Emulated OCO (TP + SL) and TWAP
- Structured logging to `bot.log`

> Important: Use the Binance Futures Testnet for development. Do not trade with real funds.

## Setup

1. Create and activate a Python 3.10+ virtual environment.
2. Copy `.env.example` to `.env` and fill your API key/secret.
3. Install dependencies.

### Quickstart (Windows PowerShell)

```powershell
python -m venv .venv; .venv\Scripts\Activate.ps1
Copy-Item .env.example .env
pip install -r requirements.txt
```

Edit `.env` and set:

```
BINANCE_API_KEY=...
BINANCE_API_SECRET=...
BINANCE_FUTURES_TESTNET=True
```

## Usage

Run each module directly with Python.

- Market order (defaults from `.env` if omitted):

```powershell
python -m src.market_orders BTCUSDT BUY 0.001
```

Dry run:

```powershell
python -m src.market_orders BTCUSDT BUY 0.001 --dry
```

- Limit order:

```powershell
python -m src.limit_orders BTCUSDT SELL 0.001 65000 --tif GTC
```

- Advanced: OCO (emulated):

```powershell
python -m src.advanced.oco BTCUSDT BUY 0.001 70000 62000
```

- Advanced: TWAP 10 slices over 5 minutes:
- Advanced: Stop-Limit:

```powershell
python -m src.advanced.stop_limit BTCUSDT BUY 0.001 65000 64000 --tif GTC --dry
```


```powershell
python -m src.advanced.twap BTCUSDT BUY 0.01 --slices 10 --duration 300
```

Logs are written to `bot.log` with timestamps and messages.

## Safe testing checklist (Testnet)

1) Verify connectivity and your credentials

```powershell
python -m src.manage ping
```

2) Place dry-run orders first to verify parameters

```powershell
python -m src.market_orders BTCUSDT BUY 0.001 --dry
python -m src.limit_orders BTCUSDT SELL 0.001 65000 --tif GTC --dry
```

3) Switch to live testnet when ready (remove --dry)

```powershell
python -m src.market_orders BTCUSDT BUY 0.001
```

4) Inspect and manage orders

```powershell
python -m src.manage open-orders BTCUSDT
python -m src.manage cancel-all BTCUSDT --dry   # preview
python -m src.manage cancel-all BTCUSDT         # execute
```

## Notes

 Input validation checks symbol format, side, and quantity > 0. Price and quantities are adjusted to exchange tick/step sizes automatically before sending.

## Project Structure
5) Configure leverage and margin type (testnet)

```powershell
python -m src.manage set-leverage BTCUSDT 10
python -m src.manage set-margin BTCUSDT ISOLATED
```

```
project_root/
├─ src/
│  ├─ market_orders.py
│  ├─ limit_orders.py
│  ├─ advanced/
│  │  ├─ oco.py
│  │  └─ twap.py
│  ├─ client.py
│  ├─ config.py
│  ├─ logger.py
│  └─ validators.py
├─ bot.log
├─ README.md
├─ requirements.txt
└─ .env.example
```
