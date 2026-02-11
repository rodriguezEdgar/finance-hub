# 🏗 Project Architecture & Directory Structure

This project follows the **Modular Monolith** pattern. Our goal is high cohesion within modules and low coupling between them.

## 📁 Directory Overview

```text
src/
├── core/                # Infrastructure & Singleton providers
├── common/              # Global utilities & Stateless helpers
├── modules/             # BUSINESS LOGIC (Domain-driven)
├── app.module.ts        # Root module (Composes everything)
└── main.ts              # Bootstrap & Global configuration

```

---

## 🏢 1. Business Modules (`src/modules/`)

This is where the actual "value" of the application lives. Each folder represents a specific business domain.

* **Structure:** Each module must be self-contained.
* `*.controller.ts`: API endpoints and request routing.
* `*.service.ts`: Business logic and data manipulation.
* `dto/`: Data Transfer Objects for input validation.
* `entities/`: Database schema definitions.


* **The Golden Rule:** Modules should only communicate via **Module Imports**. If `OrdersModule` needs `UsersService`, it must import `UsersModule`.

---

## 🛠 2. Cross-Cutting Concerns (`src/core/` & `src/common/`)

To keep business logic clean, we separate "how the app runs" from "what the app does."

### `src/core/` (Stateful/Heavy)

Contains modules that initialize connections or provide global services.

* **Database:** TypeORM/Prisma configurations.
* **Auth:** JWT strategies, guards, and session management.
* **Config:** Environment variable validation using `joi` or `class-validator`.
* **Logger:** Custom Winston or Pino implementations.

### `src/common/` (Stateless/Light)

Contains reusable code that doesn't necessarily belong to a NestJS provider.

* **Decorators:** e.g., `@CurrentUser()`, `@Public()`.
* **Filters:** Global exception handling (e.g., `HttpExceptionFilter`).
* **Interceptors:** Response transformation or execution timing.
* **Pipes:** Custom validation logic (e.g., `ParseUUIDPipe`).

---

## 🚦 Implementation Workflow

When adding a new feature (e.g., "Products"):

1. **Generate the Module:** `nest g mo modules/products`
2. **Generate Service/Controller:** `nest g s modules/products` and `nest g co modules/products`
3. **Define DTOs:** Create `src/modules/products/dto/create-product.dto.ts`.
4. **Register in App:** Ensure the module is imported in `app.module.ts`.

> **Note:** Always use **Path Aliases** (e.g., `@modules/products/...`) instead of relative paths (`../../../modules/...`) to keep the code maintainable. Check `tsconfig.json` for configured paths.

---