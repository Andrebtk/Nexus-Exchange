# Nexus — High-Performance Matching Engine

A full-stack exchange simulator featuring a high-performance Go limit-order-book matching engine (FIFO price-time priority) with REST + WebSocket APIs, PostgreSQL persistence, a live price oracle (Twelve Data), background market-maker bots, and a React trading UI.

<p align="center">
  <img src="system_architecture.svg" alt="Nexus System Architecture" width="800"/>
</p>

Live: https://nexus-trading-app.vercel.app/
## Features

### Core Trading Engine
- **Limit Order Book**: Strict FIFO price-time priority matching engine.
- **Order Management**: Market and limit orders, order queues, and real-time execution.
- **Cost-Basis & P&L**: FIFO realized P&L accounting and cost-basis tracking per user.
- **Live Price Oracle**: Real-time market data feed powered by Twelve Data.
- **Market-Maker Bots**: Automated background bots providing liquidity based on oracle prices.

### Frontend & UI
- **Modern Stack**: React 19, React Router, Vite 8.
- **Charting**: High-performance data visualization using Recharts (D3 under the hood).
- **Real-Time Data**: WebSocket integration for live order book and portfolio updates.

### Backend Infrastructure
- **Go Backend**: High-performance core (`internal/engine/exchange.go` entry point).
- **PostgreSQL**: Reliable persistence for users, orders, and transactions.
- **Architecture Diagrams**: Generated dynamically via `archi.py` (Graphviz).

## 🏗️ Architecture

The architecture revolves around a single entry point `Exchange.RouteOrder` which matches orders and settles balances.

- **OrderBook**: Composed of `bidPrices`/`askPrices` slices, routing to per-price `Limit` containers.
- **Limit**: Holds an `OrderQueue` enforcing FIFO priority.
- **Oracle**: Consumes external live price feeds for market-maker bots.

*(Architecture diagrams are generated automatically using `archi.py` as the source of truth.)*

## 🛠️ Technology Stack

- **Backend**: Go (REST + WebSocket), PostgreSQL
- **Frontend**: React 19, Vite 8, Recharts
- **Tooling**: Graphviz (`archi.py`)

## 📂 Project Structure

```text
├── cmd/api/                 # main.go — service wiring, HTTP/WS server bootstrap
├── internal/
│   ├── api/                 # REST + WS handlers (handlers.go, auth_handlers.go)
│   ├── database/            # Database connection pool (database.go)
│   ├── engine/              # Matching core (orderbook.go, limit.go, exchange.go)
│   ├── models/              # Data structures (user.go, transaction.go)
│   ├── oracle/              # Twelve Data live price feed (oracle.go)
│   └── services/            # Business logic (cost-basis, P&L, transactions)
├── nexus-ui/                # Frontend application
│   ├── src/components/      # Trading UI components
│   └── src/context/         # React context providers
├── archi.py                 # Graphviz architecture diagram generator
```

## 🚀 Getting Started

### Prerequisites
- Go 1.20+
- Node.js 18+
- PostgreSQL 14+

### Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Andrebtk/Nexus---a-high-performance-matching-engine-in-GO.git
   cd Nexus---a-high-performance-matching-engine-in-GO
   ```

2. **Database Setup:**
   ```bash
   createdb nexus
   # Run migrations/schema setup as needed
   ```

3. **Environment Configuration:**
   ```bash
   cp .env.example .env
   # Add your PostgreSQL credentials and Twelve Data API key
   ```

4. **Run Backend:**
   ```bash
   go run cmd/api/main.go
   ```

5. **Run Frontend:**
   ```bash
   cd nexus-ui
   npm install
   npm run dev
   ```

## 🚧 Known Limitations / Roadmap
- Optimization needed for O(N) slice-based price-level removal in `orderbook.go`.
- Synchronous Postgres writes inside the hot path (`RouteOrder`) need decoupling.
- Float64/uint64 mixing for currency requires normalization for precision safety.
- Hardcoded Twelve Data API key in `cmd/api/main.go` needs to be moved to environment variables.
- Hardcoded fallback user balance in `exchange.go` currently bypasses real validation.

## 📄 License
This project is licensed under the MIT License.

## Contact

For questions or support, please open an issue on GitHub.
