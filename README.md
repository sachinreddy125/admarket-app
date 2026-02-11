# AdMarket (Single App) — Ready to Deploy

This is a single Spring Boot (Java 21) app with:
- JWT auth (MARKETER / PUBLISHER / ADMIN)
- Publisher Websites + Listings
- Marketer Orders
- Escrow-style double-entry ledger (fund escrow + release to publisher)
- PostgreSQL + Flyway
- Dockerfile + docker-compose

## Run locally (Docker)
```bash
docker compose up -d --build
```

## Health check
```bash
curl http://localhost:8080/actuator/health
```

## Quick flow (example)
1) Register publisher
2) Register marketer
3) Publisher creates website + listing
4) Marketer creates order + funds
5) Publisher accepts + delivers
6) Marketer approves (releases escrow)

See API section below.

## API
### Register
POST /api/auth/register
```json
{ "email":"pub@test.com", "password":"Password123!", "role":"PUBLISHER" }
```

### Login
POST /api/auth/login
```json
{ "email":"pub@test.com", "password":"Password123!" }
```

Use token:
`Authorization: Bearer <token>`

### Publisher
- POST /api/publisher/websites?domain=example.com&niche=tech&language=en&geo=global
- GET /api/publisher/websites
- POST /api/publisher/listings?websiteId=<uuid>&title=Guest%20Post&serviceType=GUEST_POST&basePrice=120.00&currency=USD&turnaroundDays=7

### Marketplace
- GET /api/marketplace/listings

### Orders
- POST /api/orders
```json
{ "listingId":"<uuid>", "requirements":"Write about DevOps..." }
```
- POST /api/orders/{id}/fund
- POST /api/orders/{id}/accept
- POST /api/orders/{id}/deliver
- POST /api/orders/{id}/approve
- GET /api/orders/my
- GET /api/orders/inbox
