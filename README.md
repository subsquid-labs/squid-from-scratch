# squid-from-scratch

An example SQD Squid SDK indexer (a "squid") built step by step from an empty directory. It is the companion code for the [Quickstart: develop a squid from scratch](https://docs.sqd.dev/sdk/how-to-start/squid-from-scratch/) guide in the SQD documentation.

## What it does

This squid indexes [USDT](https://etherscan.io/token/0xdac17f958d2ee523a2206206994597c13d831ec7) ERC-20 `Transfer` events on Ethereum mainnet and stores them in PostgreSQL.

- `src/main.ts` configures an `EvmBatchProcessor`, points it at the SQD Network gateway for Ethereum mainnet (`https://v2.archive.subsquid.io/network/ethereum-mainnet`) plus an RPC endpoint, subscribes to `Transfer` logs from the USDT contract (`0xdAC17F958D2ee523a2206206994597C13D831ec7`), decodes them, and inserts `Transfer` records via `@subsquid/typeorm-store`.
- `schema.graphql` defines the `Transfer` entity (`id`, `from`, `to`, `value`).
- `src/model/generated/` holds the TypeORM entity classes generated from the schema.
- `src/abi/usdt.ts` holds the typed event/function bindings generated from `abi/usdt.json`.
- `db/migrations/` holds the database migration for the `transfer` table.

SQD is an open data platform for Web3. The Squid SDK is its TypeScript toolkit for extracting, transforming, and loading onchain data. See [sqd.dev](https://sqd.dev) and the [Squid SDK docs](https://docs.sqd.dev/en/sdk).

## Stack

- [`@subsquid/evm-processor`](https://www.npmjs.com/package/@subsquid/evm-processor) for batch processing of EVM data
- [`@subsquid/evm-abi`](https://www.npmjs.com/package/@subsquid/evm-abi) and `@subsquid/evm-typegen` for ABI decoding and codegen
- [`@subsquid/typeorm-store`](https://www.npmjs.com/package/@subsquid/typeorm-store), `@subsquid/typeorm-codegen`, and `@subsquid/typeorm-migration` for the database layer
- [`@subsquid/graphql-server`](https://www.npmjs.com/package/@subsquid/graphql-server) for serving a GraphQL API over the stored data
- PostgreSQL (via Docker Compose) and TypeScript

## Prerequisites

- Node.js and npm
- Docker (for the PostgreSQL database in `docker-compose.yaml`)
- An Ethereum RPC endpoint

## Configuration

Settings are read from `.env`:

```
DB_NAME=squid
DB_PORT=23798
RPC_ETH_HTTP=https://rpc.ankr.com/eth
GRAPHQL_SERVER_PORT=4350
```

Set `RPC_ETH_HTTP` to your own Ethereum RPC endpoint.

## Usage

Install dependencies:

```bash
npm install
```

Start the database:

```bash
docker compose up -d
```

The remaining steps (applying migrations, building, running the processor, and starting the GraphQL server) are described in the quickstart guide, which walks through this project command by command:

https://docs.sqd.dev/sdk/how-to-start/squid-from-scratch/

## Documentation

- Build a squid from scratch: https://docs.sqd.dev/sdk/how-to-start/squid-from-scratch/
- Squid SDK: https://docs.sqd.dev/en/sdk
- SQD docs: https://docs.sqd.dev
