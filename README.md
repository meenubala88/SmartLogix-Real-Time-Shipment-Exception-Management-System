# SmartLogix – Real-Time Shipment Exception Management System

A Spring Boot REST API starter project for managing shipments, tracking delivery status, and recording shipment exceptions.

## Features
- User registration and login with BCrypt password hashing
- JWT-based authentication for protected endpoints
- Create and list shipments
- Retrieve a shipment by ID
- Update shipment status
- Report shipment exceptions (delay, damage, address issue, customs hold, lost, other)
- Business validation: unique tracking numbers; delivered shipments cannot be changed or flagged
- MySQL persistence with Spring Data JPA / Hibernate
- Docker Compose setup for the API and MySQL

## Tech stack
Java 17, Spring Boot 3.4.5, Spring Security, JWT (JJWT), MySQL 8, Hibernate/JPA, Maven, Docker.

## Run locally
1. Install Java 17, Maven, and MySQL 8.
2. Create a database named `smartlogix` (or keep `createDatabaseIfNotExist=true`).
3. Set your database credentials if they differ from the defaults:
   - `DB_URL`
   - `DB_USERNAME`
   - `DB_PASSWORD`
4. From the project folder, run:
   ```bash
   mvn spring-boot:run
   ```
5. API base URL: `http://localhost:8080`

For a local MySQL setup, the default configuration expects `root` / `root`. Change these values to match your own machine.

## Run with Docker
```bash
docker compose up --build
```
The API will be available at `http://localhost:8080`.

## API overview

### 1. Register
`POST /api/auth/register`
```json
{
  "fullName": "Demo User",
  "email": "demo@example.com",
  "password": "StrongPass123"
}
```

### 2. Login
`POST /api/auth/login`
```json
{
  "email": "demo@example.com",
  "password": "StrongPass123"
}
```
Copy the returned token. Send it on protected requests:
`Authorization: Bearer YOUR_TOKEN`

### 3. Create shipment
`POST /api/shipments`
```json
{
  "trackingNumber": "SLX10001",
  "senderName": "ABC Traders",
  "receiverName": "Demo Customer",
  "origin": "Chennai",
  "destination": "Bengaluru"
}
```

### 4. List shipments
`GET /api/shipments`

### 5. Get one shipment
`GET /api/shipments/1`

### 6. Update status
`PATCH /api/shipments/1/status`
```json
{ "status": "IN_TRANSIT" }
```
Allowed statuses: `CREATED`, `PICKED_UP`, `IN_TRANSIT`, `OUT_FOR_DELIVERY`, `DELIVERED`, `EXCEPTION`.

### 7. Report an exception
`PATCH /api/shipments/1/exception`
```json
{
  "type": "DELAY",
  "description": "Vehicle delayed due to heavy rain."
}
```
Allowed exception types: `DELAY`, `DAMAGED`, `ADDRESS_ISSUE`, `CUSTOMS_HOLD`, `LOST`, `OTHER`.

## Important security notes
- The JWT secret in `application.properties` and `docker-compose.yml` is a development-only placeholder. Replace it with a strong, private Base64-encoded secret before deployment.
- Do not commit real passwords, API keys, tokens, or `.env` files to GitHub.
- This is a starter implementation. Add role-based access, shipment ownership/authorization, audit history, pagination, API documentation, and automated tests before production use.

## GitHub upload
Upload the contents of this project folder to your existing `SmartLogix-Real-Time-Shipment-Exception-Management-System` repository. Keep the folder structure intact. Do not upload `target/` or IDE-specific build folders.
