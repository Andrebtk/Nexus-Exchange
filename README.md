# Nexus — High-Performance Matching Engine

A full-stack exchange simulator featuring a high-performance Go limit-order-book matching engine (FIFO price-time priority) with REST + WebSocket APIs, PostgreSQL persistence, a live price oracle (Twelve Data), background market-maker bots, and a React trading UI.

<<<<<<< HEAD
Live: https://nexus-trading-app.vercel.app/
## Features
=======
<p align="center">
  <img src="system_architecture.svg" alt="Nexus System Architecture" width="800"/>
</p>

## 🚀 Features
>>>>>>> 5dad69c (cleaning)

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

<<<<<<< HEAD
1. **Start the backend**:
```bash
go run cmd/api/main.go
# API will be available at http://localhost:8080
```

2. **Start the frontend**:
```bash
cd nexus-ui
npm run dev
# Frontend will be available at http://localhost:5173
```

## Project Structure

```
.
├── cmd/                # Main applications
│   └── api/            # API server
├── internal/           # Core application code
│   ├── api/            # API handlers
│   ├── database/       # Database models and migrations
│   ├── engine/         # Matching engine
│   ├── models/         # Data models
│   ├── services/       # Business logic
│   └── oracle/         # Market data oracle
├── nexus-ui/           # React frontend
│   ├── public/        # Static assets
│   ├── src/            # React source code
│   │   ├── components/ # React components
│   │   ├── context/    # React context
│   │   └── ...         # Other frontend code
├── go.mod              # Go module definition
├── go.sum              # Go dependencies
└── README.md           # This file
```

## Key Components

### Matching Engine (`internal/engine/`)

- **OrderBook**: Manages buy/sell orders at different price levels
- **Exchange**: Routes orders to appropriate order books
- **Limit**: Price level container with double-linked list for orders
- **OrderQueue**: Efficient order queue management

### API Layer (`internal/api/`)

- RESTful API endpoints for:
  - Authentication (login, register)
  - Order management (place, cancel, history)
  - Market data (order book, tickers)
  - User portfolio (balance, positions)

### Frontend (`nexus-ui/`)

- **Profile Page**: View orders, positions, and account balance
- **Trading Interface**: Place and manage orders
- **Order Book**: View market depth
- **Authentication**: Login/registration flow

## Development

### Backend Development

```bash
# Run tests
go test ./...

# Build backend
go build -o nexus cmd/api/main.go

# Run with hot reload (using air)
air
```

### Frontend Development

```bash
cd nexus-ui
npm run dev      # Development server
npm run build    # Production build
npm run lint     # Code linting
```

## Deployment

### Docker Deployment

```bash
# Build Docker image
docker build -t nexus-trading .

# Run container
docker run -p 8080:8080 \
  -e DB_HOST=your_postgres_host \
  -e DB_USER=your_postgres_user \
  -e DB_PASSWORD=your_postgres_password \
  -e DB_NAME=nexus \
  nexus-trading
```

### Production Recommendations

- Use environment variables for configuration
- Set up proper logging and monitoring
- Implement rate limiting
- Use HTTPS with proper certificates
- Set up database backups

## API Documentation

### Authentication
- `POST /auth/register` - User registration
- `POST /auth/login` - User login
- `GET /auth/me` - Get current user

### Orders
- `POST /order` - Place new order
- `POST /orders/:id/cancel` - Cancel order
- `GET /orders/active` - Get active orders
- `GET /orders/history` - Get order history

### Market Data
- `GET /tickers` - List available tickers
- `GET /book?symbol=SYMBOL` - Get order book
- `GET /current-prices` - Get current prices

### Portfolio
- `GET /auth/profile` - User profile
- `GET /auth/stock-ownership` - Stock ownership

## Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Create a new Pull Request

## License

This project is licensed under the MIT License.

## Contact

For questions or support, please open an issue on GitHub.
=======
## 📄 License
This project is licensed under the MIT License.
>>>>>>> 5dad69c (cleaning)
