# 💰 Finance Hub

![NestJS](https://img.shields.io/badge/backend-NestJS-red)
![React](https://img.shields.io/badge/frontend-React-blue)
![Prisma](https://img.shields.io/badge/ORM-Prisma-2d3748)
![Docker](https://img.shields.io/badge/deploy-Docker-2496ed)
![License](https://img.shields.io/badge/license-MIT-green)

**Finance Hub** is a self-hosted personal finance system designed to bridge the gap between daily cash flow tracking and long-term wealth management.

Built to run on a home server (Mini-PC), it replaces complex Excel sheets with a robust database engine capable of handling the unique challenges of multi-currency economies (specifically tailored for the Argentine context with MEP/CCL rates).

### 🚀 Key Features

* **💸 Smart Expense Tracking**: Handles recurring expenses and complex Credit Card logic (installments/cuotas) to project future liabilities.
* **📈 Investment Portfolio**: Unified tracking for Stocks, CEDEARs, Bonds, ONs, and Crypto.
* **🇦🇷 Multi-Currency Engine**: Historical valuation of assets using daily exchange rates (MEP/CCL/Official) to track real Net Worth over time.
* **🤖 Automated Pricing**: Background workers (BullMQ) to scrape and update asset prices and exchange rates automatically.
* **🔒 Self-Hosted & Private**: Your financial data stays on your hardware.

### 🛠️ Tech Stack

* **Backend**: NestJS (TypeScript), BullMQ (Redis)
* **Database**: PostgreSQL + Prisma ORM
* **Frontend**: React (Vite) + Mantine/AntD
* **Infrastructure**: Docker Compose (optimized for Linux home servers)

## 🚀 Getting Started

Follow these steps to set up the project on a new machine.

### Prerequisites
- **Node.js** (Latest LTS, v20+)
- **Docker & Docker Compose** (for PostgreSQL and Redis)
- **Git**

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/finance-hub.git
cd finance-hub
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Set Up Environment Variables
- Copy the example environment file:
  ```bash
  cp backend/.env.example backend/.env
  ```
- Edit `backend/.env` and configure:
  - `DATABASE_URL`: PostgreSQL connection string (e.g., `postgresql://user:password@localhost:5432/finance_hub`)
  - `REDIS_URL`: Redis connection string (e.g., `redis://localhost:6379`)
  - Other secrets as needed

### 4. Start Infrastructure (Database & Redis)
```bash
docker-compose up -d
```
This starts PostgreSQL and Redis containers.

### 5. Set Up Database Schema
```bash
npx nx run backend:prisma-migrate
```
This applies Prisma migrations to set up the database schema.

### 6. Run the Applications
- **Backend** (in one terminal):
  ```bash
  npx nx serve backend
  ```
- **Frontend** (in another terminal):
  ```bash
  npx nx serve frontend
  ```

The app will be available at:
- Frontend: http://localhost:4200
- Backend API: http://localhost:3000

### 🧪 Running Tests
- Backend: `npx nx test backend`
- Frontend: `npx nx test frontend`
- All: `npx nx run-many --target=test`

### 📦 Building for Production
- Backend: `npx nx build backend`
- Frontend: `npx nx build frontend`
- All: `npx nx run-many --target=build`
