# API Documentation

This directory contains the API endpoints for the Trading AI Booster server. The API is built using Flask and provides access to accounts, trades, market data, and system diagnostics.

## 📚 Table of Contents

- [Accounts](#accounts)
- [Symbols & Config](#symbols-config)
- [Trades](#trades)
- [Candles](#candles)
- [Ticks](#ticks)
- [Announcements](#announcements)
- [Signals & AI](#signals-ai)
- [Statistics & Data](#statistics-data)
- [System Logs](#system-logs)
- [Real-time Stream](#real-time-stream)
- [Telegram](#telegram)

---

## <a name="accounts"></a> 🏦 Accounts

Manage trading accounts and retrieve account information.

### `GET /accounts`
Retrieve a list of all registered trading accounts.
- **Response:** Array of account objects.

### `GET /account`
Retrieve detailed information for a specific account.
- **Query Params:** `account_number`
- **Response:** Detailed account object.

### `POST /accounts/update`
Update or create account information (from MT5).
- **Body:** `account_number`, `balance`, `equity`, `margin`, `free_margin` `broker`, `currency`, etc.

---

## <a name="symbols-config"></a> ⚙️ Symbols & System Config

### `GET /symbols`
Get a list of all available symbols.

### `GET /symbol-config`
Get enabled symbols from the configuration.

### `GET /system-config`
Get system configuration parameters.

### `POST /system-config`
Update or create a system configuration key-value pair.
- **Body:** `key`, `value`, `category`, `description`

### `DELETE /admin/symbolconfig/delete/`
Delete a symbol configuration (Admin only).
- **Query Params:** `id`

---

## <a name="trades"></a> 🔄 Trades

Trade history and management.

### `GET /trades`
Get a list of trades.
- **Query Params:** `state` ('open', 'history'), `account_number`

### `POST /trades`
Create or update a single trade.
- **Body:** `ticket`, `symbol`, `type`, `volume`, `open_price`, `open_time`, etc.

### `POST /trades/batch`
Batch create/update trades.
- **Body:** `trades` (Array of trade objects)

### `PUT /trades/<id>`
Update a specific trade by internal ID.
- **Body:** Fields to update.

### `POST /trades/<id>/rating`
Update user rating for a trade.
- **Body:** `rating` (0-5)

### `GET /trade-notes`
Get trade notes.
- **Query Params:** `trade_ticket`

### `POST /trade-notes`
Create a trade note.
- **Body:** `trade_ticket`, `note_content`, `note_title`

### `DELETE /trade-notes/<id>`
Delete a trade note.

---

## <a name="candles"></a> 🕯️ Candles

Historical chart data.

### `GET /candles`
Retrieve historical candle data.
- **Query Params:** `symbol`, `timeframe`, `limit`, `before`

### `GET /candles/last`
Get timestamp of the last recorded candle.
- **Query Params:** `symbol`, `timeframe`

### `POST /candles`
Bulk add candles.
- **Body:** `symbol`, `timeframe`, `candles` (Array)

---

## <a name="ticks"></a> 📉 Ticks

High-frequency tick data.

### `GET /ticks`
Get recent ticks.
- **Query Params:** `symbol`, `limit`

### `POST /ticks`
Add a single tick.
- **Body:** `symbol`, `bid`, `ask`

### `POST /ticks/batch`
Batch add ticks (Ultra-fast Redis ingestion).
- **Body:** `ticks` (Array)

### `POST /ticks/aggregate`
Aggregate ticks into a candle for a specific timeframe.
- **Body:** `symbol`, `timeframe`

### `GET /ticks/stream`
Stream ticks via Server-Sent Events (SSE).

---

## <a name="announcements"></a> 📢 Announcements

Economic calendar and news processing.

### `GET /announcements`
Get announcements.
- **Query Params:** `status`, `search`, `page`, `filter_type`

### `POST /announcements/new`
**Core Automation Endpoint**: Receives raw announcement text, analyzes it with DeepSeek AI, creates a trading signal if confidence is high, and sends Telegram notifications.
- **Body:** `announcement_text` (Required), `symbol`, `source_type`

### `POST /announcements/<id>/process`
Mark an announcement as processed manually.

### `GET /announcements/stats`
Get statistics about announcement analysis (Total, Today, Buy/Sell counts).

---

## <a name="signals-ai"></a> 🚦 Signals & AI

Trading signals and AI reasoning.

### `GET /trading-signals`
Get a list of trading signals.
- **Query Params:** `status`

### `GET /trading-signals/pending`
Get pending signals for execution (used by defined terminals).

### `GET /signals`
Get all signals (alternative endpoint).

### `POST /signals`
Create or update a signal manually.

### `POST /trading-signals/<id>/update`
Update signal status (e.g., 'executed', 'rejected').
- **Body:** `status`, `trade_ticket`

### `POST /signals/<signal_id>/work`
Mark a signal as "in progress" (Work status).

### `GET /ai-signals`
Get AI signals with visualization data (time, entry, confidence).
- **Query Params:** `symbol`, `days`

### `GET /ai-responses`
Get raw AI analysis responses.
- **Query Params:** `announcement_id`

---

## <a name="statistics-data"></a> 📊 Statistics & Data

### `GET /full-stats`
Comprehensive dashboard statistics (Capital, Risk, Portfolio, AI breakdown).

### `GET /statistics/data`
Advanced statistics including correlations and detailed candle stats.
- **Query Params:** `correlation_timeframe`, `correlation_candles`, `min_confidence`, `max_confidence`

### `GET /statistics/recommendations`
Get AI-driven improvement recommendations based on stats.

---

## <a name="system-logs"></a> 🖥️ System Logs

### `GET /system-logs`
Basic system logs.

### `GET /logs/enhanced`
Filtered logs.
- **Query Params:** `level`, `module`, `limit`

### `GET /logs/stats`
Log statistics.

### `GET /logs/errors`
Recent errors and critical alerts.

### `POST /logs/cleanup`
Delete old logs.
- **Body:** `days`

---

## <a name="real-time-stream"></a> 📡 Real-time Stream

### `GET /stream/all`
**Universal Stream**: SSE endpoint streaming Ticks, Candles, Trades, and Account updates in real-time.

---

## <a name="telegram"></a> 📱 Telegram

### `GET /telegram/users`
List configured Telegram users.

### `POST /telegram/send`
Send custom message.
- **Body:** `recipients`, `message`, `image_url`

### `POST /telegram/upload-image`
Upload image for Telegram.
- **Form Data:** `image`

### `POST /telegram/webhook`
Handle incoming Telegram updates (Webhooks).
