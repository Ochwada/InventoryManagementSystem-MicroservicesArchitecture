# 🛒 Inventory Management System-Microservices Architecture
The **Inventory Management System** is a modular, containerized application built with **Spring Boot** and **Docker**
designed to streamline product management, inventory tracking, and order processing in retail or warehouse environments.
It follows a microservices architecture, ensuring that each component is scalable, maintainable, and independently deployable.

## 📌 Overview

This system consists of 5  microservices:

| Microservice                                                                                  | Role / Service #               | Database   | Port | Status         |
|-----------------------------------------------------------------------------------------------|--------------------------------|------------|------|----------------|
| [**Authentication  Service**](https://github.com/Ochwada/MicroInventorySystem-Authentication) | Microservice 0: Authentication |            | 9099 | ✅ Done         |
| [**Product Service**](https://github.com/Ochwada/MicroInventorySystem-Product)                | Microservice 1: Product        | PostgreSQL | 9090 | ✅ Done         |
| [**Inventory Service**](https://github.com/Ochwada/MicroInventorySystem-Inventory)            | Microservice 2: Inventory      | MongoDB    | 9091 | ✅ Done         |
| [**Order Service**](https://github.com/Ochwada/MicroInventorySystem-Order)                    | Microservice 3: Order          | MongoDB    | 9092 | ✅ Done         |
| [**Notification Service**](https://github.com/Ochwada/MicroInventorySystem-Notification)      | Microservice 4: Notification   | MongoDB    | 9093 | 🚧 In Progress |


Each service is independently deployable and communicates over REST APIs. Docker is used for containerization and 
orchestration is done using **Docker Compose**.

## 📁 Project Structure
``` bash
MicroInventorySystem/
│
├── product-service/ # Product catalog (PostgreSQL)
├── inventory-service/ # Inventory tracking (MongoDB)
├── order-service/ # Order management (MongoDB)
├── notification-service/ # Notification management (MongoDB)
├── authentication-Service/ # Authetication management 
│
├── docker-compose.yml # Multi-service Docker orchestration
└── README.md # Project documentation

```

## 🖥️ The Microservices Architecture
The system is composed of several specialized services, each responsible for a specific domain:

### 0️⃣ Microservice 0: Authentication  Service
🖇️ [Git Repository : Authentication Service](https://github.com/Ochwada/MicroInventorySystem-Authentication)

**Purpose**: Handles user authentication, registration, and authorization across the system.

#### Some Functions
- Register new users and store credentials securely (e.g., hashed passwords)
- Authenticate users and issue JWT tokens for secure access 
- Verify tokens for protected routes and microservices 
- Assign user roles (e.g., ADMIN, USER) and manage access levels 
- Integrate with other services (like Product or Inventory) via token-based authorization

### 1️⃣ Microservice 1: Product Service
🖇️ [Git Repository : Product Service](https://github.com/Ochwada/MicroInventorySystem-Product)
gi
**Purpose**: Manages product catalog and metadata.

#### Some Functions
- Store and retrieve product details: ID, name, description, price
- Communicate with `Inventory Service` to check stock quantity


### 2️⃣ Microservice 2: Inventory Service
🖇️ [Git Repository : Inventory Service](https://github.com/Ochwada/MicroInventorySystem-Inventory)
**Purpose**: Maintains stock levels for each product and updates inventory on product movement.
#### Some Functions
- Query available stock for a given product ID.
- Increase or decrease stock quantities (e.g., during restocking or order placement).
- Communicates with Order Service to validate stock availability before order placement.


### 3️⃣ Microservice 3: Order  Service
🖇️ [Git Repository : Order Service](https://github.com/Ochwada/MicroInventorySystem-Order)
**Purpose**: Manages the lifecycle of customer orders.
#### Some Functions
- Accepts and stores customer orders
- Checks stock availability via Inventory Service
- Updates inventory if order is confirmed

###  4️⃣ Microservice 4: Order  Service
🖇️ [Git Repository : Notification Service](https://github.com/Ochwada/MicroInventorySystem-Notification)
**Purpose**: Handles key events like order confirmations and stock alerts by sending simulated notifications via log output.
#### Some Functions
- Sends order confirmation logs to users. 
- Logs out-of-stock alerts for admins.

---

## 🚀 Getting Started

### Prerequisites
- Docker + Docker Compose installed
- Git installed
- Java 17+ and Maven (for local builds, if needed)

### Step 1: Clone the Repositories

```bash 


git clone https://github.com/Ochwada/MicroInventorySystem-Authentication authentication-Service
git clone https://github.com/Ochwada/MicroInventorySystem-Product.git product-service
git clone https://github.com/Ochwada/MicroInventorySystem-Inventory.git inventory-service
git clone https://github.com/Ochwada/MicroInventorySystem-Order.git order-service
git clone https://github.com/Ochwada/MicroInventorySystem-Notification.git notification-service


```

### Step 2: Directory Layout
Ensure your directory looks like this:

```
MicroInventorySystem/
├── docker-compose.yml
├── .gitignore
├── README.md
│
├── authentication-service/
├── product-service/
├── inventory-service/
├── notification-service/
└── order-service/
```

### Step 3: Environment Variables / Environment Configurations
Each service includes a `.env` file with required configuration:

- `authentication-service/.env`
```.dotenv
#-------------------------------------------
#  Configuration
#-------------------------------------------
GOOGLE_CLIENT_ID=google_client_id
GOOGLE_CLIENT_SECRET=google_secret
```


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
