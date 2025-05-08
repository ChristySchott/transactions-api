# 💰 Transactions API

A RESTful API built with **Fastify**, **Knex**, and **TypeScript** to manage user transactions. It features strong validation, seamless database migrations, and comprehensive integration tests, and is deployed on **Render** for cloud hosting.

## 🚀 Features

- **Create Transaction** — Users can create a new transaction.  
- **Account Summary** — Retrieve a summary of the user's balance (credits minus debits).  
- **List Transactions** — Fetch all transactions for the authenticated user.  
- **Get Transaction** — View details of a single transaction by its ID.  

### Business Rules

- Transactions are either **credit** (adds to the balance) or **debit** (subtracts from the balance).  
- Each request is **authenticated**, carrying user identity via secure cookies.  
- Users can only access their own transactions.  

## 🧰 Tech Stack

- **Runtime & Frameworks**  
  - Node.js 20  
  - TypeScript  
  - Fastify  
  - Knex (SQL query builder & migrations)  

- **Database**  
  - SQLite3 (development)  
  - PostgreSQL (optional, via `pg` driver)  

- **Validation & Security**  
  - Zod for schema validation  
  - `@fastify/cookie` for cookie handling  
  - `dotenv` for environment variables  

- **Testing**  
  - Vitest for unit & integration tests  
  - Supertest for HTTP assertions  

- **Tooling**  
  - TSX for TypeScript execution  
  - TSUP for bundling  
  - ESLint & Prettier for code quality  

## 🗄️ Database & Migrations

- Migrations are stored under `db/migrations` and managed via Knex.  
- By default uses SQLite3 (`.env` sets `DATABASE_URL` to a file path).  
- Can be pointed to PostgreSQL by updating `DATABASE_CLIENT` and `DATABASE_URL` in `.env`.  

## 📂 Project Structure

```plain
├── db/
│   └── migrations/       # Knex migration files
├── test/                 # Integration & unit tests
└── src/
    ├── @types/           # Custom TypeScript types
    ├── middlewares/      # Fastify hooks
    ├── routes/           # Route definitions
    ├── env/              # Environment variable loader
    ├── database.ts       # Knex configuration
    ├── app.ts            # Fastify app configurations
    └── server.ts         # Server

├── .env                  # Environment variables
├── package.json
├── tsconfig.json
└── README.md
```

## 🧪 Testing

- All tests (unit & integration) live in the `test/` directory.  
- Uses **Vitest** as the test runner and **Supertest** for HTTP assertions.  
- Run the full suite with:

```bash
  npm run test
```

## 👨‍💻 Developer Notes

- **Layered Architecture**  
  - `routes/` defines all HTTP endpoints  
  - `middlewares/` handles validation, authentication, and error handling  
  - `database.ts` configures Knex and runs migrations  
  - `app.ts` wires Fastify plugins, decorators, and hooks  

- **Type Safety**  
  - Full TypeScript coverage using interfaces and Zod schemas  
  - Runtime validation via Zod, with compile-time inference  

- **Fastify Best Practices**  
  - Use decorators (e.g., `request.user`) for shared utilities  
  - Leverage hooks (`onRequest`, `preHandler`, `onSend`) for cross-cutting concerns  
  - Register plugins modularly to keep `app.ts` clean  

- **Migration Strategy**  
  - Always run `npm run knex -- migrate:latest` before starting the server  
  - Write small, incremental migrations in `db/migrations`  
  - Ensure each migration is reversible for clean rollbacks  

- **Testing Tips**  
  - Use an in-memory SQLite instance for fast integration tests  
  - Reset DB state between tests with Knex’s `migrate:rollback` & `migrate:latest`  
  - Mock external services where possible to isolate API behavior
