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
