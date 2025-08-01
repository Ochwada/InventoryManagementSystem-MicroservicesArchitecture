# 🛒 Inventory Management System-Microservices Architecture
The **Inventory Management System** is a modular, containerized application built with **Spring Boot** and **Docker**
designed to streamline product management, inventory tracking, and order processing in retail or warehouse environments.
It follows a microservices architecture, ensuring that each component is scalable, maintainable, and independently deployable.

## 📌 Overview

This system consists of 3 core microservices:

| Microservice          | Tech Stack                       | Database   | Port |
|-----------------------|----------------------------------|------------|------|
| **Product Service**   | Spring Boot, Spring Data JPA     | PostgreSQL | 9090 |
| **Inventory Service** | Spring Boot, Spring Data MongoDB | MongoDB    | 9091 |
| **Order Service**     | Spring Boot, Spring Data MongoDB | MongoDB    | 9092 |

Each service is independently deployable and communicates over REST APIs. Docker is used for containerization and 
orchestration is done using **Docker Compose**.

## 📁 Project Structure
``` bash
MicroInventorySystem/
│
├── product-service/ # Product catalog (PostgreSQL)
├── inventory-service/ # Inventory tracking (MongoDB)
├── order-service/ # Order management (MongoDB)
│
├── docker-compose.yml # Multi-service Docker orchestration
└── README.md # Project documentation

```

## 🖥️ The Microservices Architecture
The system is composed of several specialized services, each responsible for a specific domain:

### 🔹 Microservice 1: Product Service

🖇️ [Git Repository : Product Service](https://github.com/Ochwada/MicroInventorySystem-Product)

**Purpose**: Manages product catalog and metadata.

#### Some Features

- Store and retrieve product details: ID, name, description, price
- Communicate with `Inventory Service` to check stock quantity


### 🔹 Microservice 2: Inventory Service

🖇️ [Git Repository : Inventory Service](https://github.com/Ochwada/MicroInventorySystem-Inventory)

**Purpose**: Maintains stock levels for each product and updates inventory on product movement.

#### Some Features

- Query available stock for a given product ID.
- Increase or decrease stock quantities (e.g., during restocking or order placement).
- Communicates with Order Service to validate stock availability before order placement.


### 🔹 Microservice 3: Order  Service

🖇️ [Git Repository : Order Service](https://github.com/Ochwada/MicroInventorySystem-Order)

**Purpose**: Manages the lifecycle of customer orders.

#### Some Features

- Accepts and stores customer orders
- Checks stock availability via Inventory Service
- Updates inventory if order is confirmed

---

## 🚀 Getting Started

### Prerequisites
- Docker + Docker Compose installed
- Git installed
- Java 17+ and Maven (for local builds, if needed)

### Step 1: Clone the Repositories

```bash 
git clone https://github.com/Ochwada/MicroInventorySystem-Product.git product-service
git clone https://github.com/Ochwada/MicroInventorySystem-Inventory.git inventory-service
git clone https://github.com/Ochwada/MicroInventorySystem-Order.git order-service
```

### Step 2: Directory Layout
Ensure your directory looks like this:

```
MicroInventorySystem/
├── docker-compose.yml
├── product-service/
├── inventory-service/
└── order-service/
```

### Step 3: Environment Variables / Environment Configurations
Each service includes a `.env` file with required configuration:


- `product-service/.env`
```.dotenv
#-------------------------------------------
# connection Configuration
#-------------------------------------------

POSTGRES_DB=database_name
POSTGRES_USER=database_username
POSTGRES_PASSWORD=database_password

#inventory http
INVENTORY_SERVICE_URL=url_to_the_inventory
```


- `inventory-service/.env`
```.dotenv
#-------------------------------------------
# PORT configuration
#-------------------------------------------
PORT=service_port

#-------------------------------------------
# MongoDB configuration
#-------------------------------------------
SPRING_DATA_MONGODB_URI=url_to_the_mongo_database

```


- `order-service/.env`
```.dotenv
#-------------------------------------------
# PORT configuration
#-------------------------------------------
PORT=service_port

#-------------------------------------------
# MongoDB configuration
#-------------------------------------------
SPRING_DATA_MONGODB_URI=url_to_the_mongo_database

#-------------------------------------------
#Inventory service base url (for InventoryClient) - http
#-------------------------------------------
INVENTORY_SERVICE_URL=url_to_the_inventory
```
### Step 4: Build and Run the System
Make sure the docker-compose.yml file in this repo is in the root directory. 

From the root directory, run:
```bash
docker-compose up --build
```
This will:

- Build all services using multi-stage Dockerfiles 
- Start PostgreSQL and MongoDB containers 
- Start all 3 microservices

**To stop:**
```bash
docker-compose down
```

**To clean and start the docker images**
```bash
docker-compose down --volumes --remove-orphans 

docker-compose up --build
```
## Endpoints Overview
| Service   | Endpoint Examples                                     |
|-----------|-------------------------------------------------------|
| Product   | `GET http://localhost:9090/products`                  |
| Inventory | `GET http://localhost:9091/api/inventory/{productId}` |
| Order     | `POST http://localhost:9092/orders`                   |

## Data Persistence

- MongoDB data is stored in mongo_data volume 
- PostgreSQL data is stored in pg_data volume

You can verify volumes using:
```bash
docker volume ls

```
