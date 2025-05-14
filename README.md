
# FullCycle Auction Service

This Go service implements an auction system with **automatic closure** after a configurable time set via an environment variable.

---

## Features

- Create auctions with product details
- Automatic closure after a defined time
- Valid bids only while the auction is **open**
- Database: MongoDB
- Observability through logs (Logrus)

---

## Environment Variables

Create a `.env` file in `cmd/auction/.env` with the following variables:

```env
# Auction duration (e.g., 30s, 2m, 1h)
AUCTION_INTERVAL=30s
# MongoDB configuration
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=admin
MONGODB_URL=mongodb://admin:admin@mongodb:27017/auctions?authSource=admin
MONGODB_DB=auctions
```

---

## Running with Docker

1. Clone this repository:

    ```bash
    git clone https://github.com/HenriqueOtsuka/labs-auction-goexpert
    cd labs-auction-goexpert
    ```

2. Update the `.env` file as shown above.

3. Start the project using Docker Compose:

    ```bash
    docker compose up --build -d
    ```

4. Access the application at `http://localhost:8080`.

---

## Running Tests

To execute automated tests (including automatic closure tests):

```bash
docker compose exec app go test ./... -v
```

Or test the auction package directly:

```bash
docker compose exec app go test ./internal/infra/database/auction -v
```

---

## Automatic Auction Closure

The automatic closure is based on the time configured in the `AUCTION_INTERVAL` variable.

Each created auction triggers a **goroutine** that:
- Waits for the configured duration
- Marks the auction as `Completed` in the database

You can verify this behavior by running the included test:

```bash
docker compose exec app go test ./internal/infra/database/auction -v
```

---

## Stopping the Application

To stop and clean up all resources:

```bash
docker compose down -v
```
