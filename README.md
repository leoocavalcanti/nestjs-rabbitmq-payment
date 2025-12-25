# 🛒 NestJS Microservice with RabbitMQ

![NestJS](https://img.shields.io/badge/nestjs-%23E0234E.svg?style=for-the-badge&logo=nestjs&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![MySQL](https://img.shields.io/badge/mysql-%2300f.svg?style=for-the-badge&logo=mysql&logoColor=white)
![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white)

A modern e-commerce checkout microservice built with **NestJS**, featuring asynchronous payment processing through **RabbitMQ** message queues and **MySQL** database persistence.

## 🎯 Project Overview

This microservice demonstrates a real-world e-commerce checkout system with:

- **Checkout Management**: Create and manage shopping cart checkouts
- **Asynchronous Payment Processing**: Payment events handled via RabbitMQ queues
- **Database Persistence**: MySQL with TypeORM for data storage
- **Modular Architecture**: Clean separation of concerns with NestJS modules

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Checkout API  │───▶│   RabbitMQ      │───▶│ Payment Service │
│                 │    │   Message       │    │                 │
│   - Create      │    │   Queue         │    │ - Process       │
│   - List        │    │                 │    │ - Confirm       │
│   - Pay/Fail    │    │                 │    │ - Reject        │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                                              │
         ▼                                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    MySQL Database                               │
│                   (Checkout Data)                              │
└─────────────────────────────────────────────────────────────────┘
```

## 🚀 Quick Start with Docker

### Prerequisites

- **Docker** and **Docker Compose** installed on your system
- **Node.js 18+** (for local development)

### 1. Clone the Repository

```bash
git clone <repository-url>
cd live-imersao-17-nestjs-rabbitmq
```

### 2. Start Infrastructure Services

Start MySQL and RabbitMQ containers:

```bash
docker-compose up -d
```

This will start:
- **MySQL** on port `3306` (user: `root`, password: `root`, database: `nest`)
- **RabbitMQ Management** on port `15672` (user: `admin`, password: `admin`)
- **RabbitMQ AMQP** on port `5672`

### 3. Install Dependencies

```bash
npm install
```

### 4. Run the Application

```bash
# Development mode with auto-reload
npm run start:dev

# Production mode
npm run start:prod
```

The API will be available at: `http://localhost:3000`

## 🔧 Available Services

### MySQL Database
- **URL**: `localhost:3306`
- **Database**: `nest`
- **Username**: `root`
- **Password**: `root`

### RabbitMQ Management UI
- **URL**: `http://localhost:15672`
- **Username**: `admin`
- **Password**: `admin`

### RabbitMQ AMQP
- **URL**: `amqp://localhost:5672`
- **Username**: `admin`
- **Password**: `admin`

## 📚 API Endpoints

### Checkouts

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/checkouts` | Create a new checkout |
| `GET` | `/checkouts` | List all checkouts |
| `GET` | `/checkouts/:id` | Get checkout by ID |
| `PATCH` | `/checkouts/:id/pay` | Mark checkout as paid |
| `PATCH` | `/checkouts/:id/fail` | Mark checkout as failed |

### Example: Create Checkout

```bash
curl -X POST http://localhost:3000/checkouts \
  -H "Content-Type: application/json" \
  -d '{
    "items": [
      {
        "product_id": 1,
        "quantity": 2
      },
      {
        "product_id": 3,
        "quantity": 1
      }
    ]
  }'
```

## 🛠️ Development

### Available Scripts

```bash
# Development
npm run start:dev      # Start with auto-reload
npm run start:debug    # Start in debug mode

# Testing
npm run test           # Run unit tests
npm run test:watch     # Run tests in watch mode
npm run test:cov       # Run tests with coverage
npm run test:e2e       # Run end-to-end tests

# Code Quality
npm run lint           # Run ESLint
npm run format         # Format code with Prettier

# Build
npm run build          # Build for production
npm run start:prod     # Start production server
```

### Project Structure

```
src/
├── checkouts/          # Checkout module
│   ├── dto/           # Data Transfer Objects
│   ├── entities/      # TypeORM entities
│   ├── checkouts.controller.ts
│   ├── checkouts.service.ts
│   └── checkouts.module.ts
├── payments/          # Payment processing module
│   ├── payment.consumer.ts
│   └── payments.module.ts
├── rabbitmq/          # RabbitMQ configuration
│   └── rabbitmq.module.ts
├── app.module.ts      # Root module
└── main.ts           # Application entry point
```

## 🔄 Message Queue Flow

1. **Checkout Created**: When a checkout is created, a message is published to `checkout.created` queue
2. **Payment Processing**: The payment consumer listens for checkout events
3. **Status Update**: Payment results update the checkout status in the database

### Queue Configuration

- **Exchange**: `amq.direct`
- **Routing Key**: `checkout.created`
- **Consumer**: Payment service processes checkout events

## 🐳 Docker Commands

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Rebuild and start
docker-compose up -d --build

# Access MySQL CLI
docker-compose exec db mysql -u root -p nest

# Access RabbitMQ container
docker-compose exec rabbitmq bash
```

## 🧪 Testing

The project includes comprehensive testing setup:

```bash
# Run all tests
npm test

# Watch mode for development
npm run test:watch

# Coverage report
npm run test:cov

# E2E tests
npm run test:e2e
```

## 📊 Monitoring

### RabbitMQ Management Dashboard

Access the RabbitMQ management interface at `http://localhost:15672` to:

- Monitor queue metrics
- View message flows
- Manage exchanges and queues
- Debug message routing

### Database Monitoring

Connect to MySQL to view checkout data:

```bash
docker-compose exec db mysql -u root -p nest
```

```sql
-- View all checkouts
SELECT * FROM checkout;

-- View checkout items
SELECT * FROM checkout_item;
```

## 🔒 Security Notes

This is a development setup. For production:

- Change default passwords
- Use environment variables for sensitive data
- Implement proper authentication
- Configure SSL/TLS for database connections
- Set up proper network security

---
