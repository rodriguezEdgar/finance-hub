# Agent Context & Rules: Finance Hub

## 1. Project Overview
"Finance Hub" is a personal, self-hosted wealth management system.
- **Goal:** Replace complex Excel sheets (Fixed Expenses & Investments).
- **Core Domains:**
  1. **Cash Flow:** Recurring expenses, credit card installments (Cuotas), income.
  2. **Net Worth:** Asset tracking (Stocks, Bonds, Crypto), historical MEP/CCL valuation.
- **Infrastructure:** Runs on a Linux Mini-PC via Docker Compose.

## 2. Tech Stack (Strict)
- **Runtime:** Node.js (Latest LTS).
- **Language:** TypeScript (Strict Mode).
- **Backend Framework:** NestJS (Modules, Decorators, Dependency Injection).
- **Database:** PostgreSQL (v15+).
- **ORM:** Prisma (Schema-first design).
- **Queue:** BullMQ + Redis (For scraping jobs).
- **Frontend:** React + Vite + Mantine (or AntD).
- **Monorepo:** Nx (For managing frontend and backend in a single repository).

## 3. Coding Standards & Patterns
### General
- **Strict Types:** No `any`. Define Interfaces or DTOs for everything.
- **Environment:** Use `@nestjs/config` for env vars. Never hardcode secrets.

### Backend (NestJS)
- **Architecture:** Controller -> Service -> Prisma Repository.
- **Validation:** Use `class-validator` and `class-transformer` for all DTOs.
- **Error Handling:** Use global filters. Return standard HTTP exceptions.
- **Math:** For financial calculations (Money), use `decimal.js` or Prisma's `Decimal` type. Never use standard JS `float` for currency.

### Database (Prisma)
- Use `UUID` for all primary keys.
- Use `camelCase` for fields and `PascalCase` for models.
- Enums are preferred for fixed lists (Currency, AssetType).

## 4. Domain Specific Rules
- **Currency:** All monetary values must have an associated `Currency` enum (ARS/USD).
- **Argentine Context:** The app heavily relies on "Exchange Rates" (MEP/CCL). When converting values, always check the Date of the transaction to pick the historic rate.
- **Cuotas:** Expenses can be installments. If `totalInstallments` > 1, the logic must project future expenses.