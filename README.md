# Trading AI Booster v3.5.0

A comprehensive, high-performance trading operating system powered by AI and real-time data analysis.

[![Alt text](https://img.youtube.com/vi/VID/0.jpg)](https://www.youtube.com/watch?v=GgXTqxk_BHE)

## Overview

Trading AI Booster is designed to automate trading analysis, manage risk, and execute trades based on economic announcements and technical indicators. It features a robust backend, a real-time dashboard, and integration with MetaTrader 5 via MQ5 scripts.

### Key Features
- **AI Integration**: DeepSeek AI analysis for economic news and technical patterns.
- **Real-time Data**: Redis-powered streaming for ticks and candles.
- **Automated Execution**: Order management and risk control.
- **Interactive Dashboard**: Real-time visualization and control center.

## Installation

### Prerequisites
- Postgresql
- Python 3.8+
- Redis Server
- MetaTrader 5 (for data ingestion)

### Setup

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd trading_AI_booster_v350
    ```

2.  **Install dependencies:**
    ```bash
    pip install -r server/requirements.txt
    ```

3.  **Configure Environment:**
    Create a `.env` file in the `server` directory (or root) based on the example below.

4.  **Initialize Database:**
    ```bash
    python server/init_db.py
    ```

## Configuration

The system is configured via environment variables in a `.env` file.

### Core Settings
| Variable | Description | Default |
|----------|-------------|---------|
| `FLASK_APP` | Entry point for Flask | `server/app.py` |
| `FLASK_ENV` | Environment mode | `development` |
| `SECRET_KEY` | Flask secret key | `dev-key-please-change` |
| `DASHBOARD_PORT` | Port for the dashboard | `5000` |

### Database & Redis
| Variable | Description | Default |
|----------|-------------|---------|
| `DATABASE_URL` | SQLite/Postgres connection string | `sqlite:///trading_os.db` |
| `REDIS_URL` | Redis connection string | `redis://localhost:6379/0` |

### AI & APIs
| Variable | Description |
|----------|-------------|
| `DEEPSEEK_API_KEY` | API Key for DeepSeek AI |
| `TELEGRAM_BOT_TOKEN` | Token for Telegram notifications |
| `TELEGRAM_CHAT_ID` | Chat ID for Telegram notifications |

## Usage

### Starting the Server
Run the application using the provided run script:

```bash
python run.py
```

The server will start at `http://localhost:5000`.

### Accessing the Interface
- **Dashboard**: `http://localhost:5000/dashboard`
- **Admin Panel**: `http://localhost:5000/admin/`
- **Documentation**: `http://localhost:5000/docs`

### MetaTrader 5 Integration
1.  Copy the scripts from the `MQ5` directory to your MT5 `MQL5/Scripts` folder.
2.  Attach `CandleDataSender.mq5` to the charts you wish to monitor.


