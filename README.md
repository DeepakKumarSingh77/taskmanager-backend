# Task Manager REST API

A backend-focused Task Management REST API built with **Java 17 and Spring Boot 3**, featuring JWT authentication, MySQL persistence, Redis caching, RabbitMQ messaging, validation, and OpenAPI/Swagger documentation.

The project demonstrates a production-style backend architecture with secure authentication, database integration, caching, asynchronous messaging, and RESTful API design.

## 🚀 Features

- 🔐 **JWT Authentication**
  - User registration and login
  - Stateless authentication using JSON Web Tokens
  - Protected task APIs

- 📋 **Task Management**
  - Create, read, update, and delete tasks
  - Task status and priority management
  - Due dates and attachment URL support
  - Pagination and sorting

- 🗄️ **MySQL + Spring Data JPA**
  - Relational database persistence
  - Entity mapping using JPA/Hibernate
  - Automatic schema updates during development

- ⚡ **Redis Caching**
  - Cache frequently accessed task data
  - Cache eviction on update/delete operations
  - Reduces unnecessary database queries

- 📨 **RabbitMQ Messaging**
  - Asynchronous task-related notifications
  - Producer/consumer messaging architecture
  - Decouples background processing from API requests

- 🛡️ **Spring Security**
  - JWT authentication filter
  - Secured API endpoints
  - Request authorization

- ✅ **Validation & Error Handling**
  - Request validation using Jakarta Validation
  - Centralized exception handling

- 📚 **API Documentation**
  - OpenAPI/Swagger integration
  - Interactive API testing through Swagger UI

- 🐳 **Docker**
  - Redis container
  - RabbitMQ container with management UI

---

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| Language | Java 17 |
| Framework | Spring Boot 3.2 |
| REST API | Spring Web |
| Security | Spring Security + JWT |
| ORM | Spring Data JPA + Hibernate |
| Database | MySQL |
| Caching | Redis |
| Messaging | RabbitMQ |
| API Documentation | OpenAPI / Swagger |
| Build Tool | Maven |
| Containerization | Docker |
| Testing/API Client | Postman |
| Version Control | Git & GitHub |

---

## 🏗️ Architecture

```text
                    ┌──────────────────┐
                    │     Client       │
                    │ Postman / Swagger│
                    └────────┬─────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │   Spring Boot API   │
                  │                     │
                  │ Controllers         │
                  │ Services            │
                  │ Security / JWT      │
                  └─────────┬───────────┘
                            │
              ┌─────────────┼──────────────┐
              │             │              │
              ▼             ▼              ▼
        ┌──────────┐   ┌──────────┐  ┌───────────┐
        │  MySQL   │   │  Redis   │  │ RabbitMQ  │
        │ Database │   │  Cache   │  │ Messaging │
        └──────────┘   └──────────┘  └───────────┘
```

### Request Flow

```text
Client
  ↓
Controller
  ↓
JWT Authentication
  ↓
Service Layer
  ↓
Repository Layer
  ↓
MySQL

Frequently accessed data
  ↓
Redis Cache

Background notification
  ↓
RabbitMQ
  ↓
Message Consumer
```

---

## 📁 Project Structure

```text
spring-taskmanager-backend/
│
├── src/
│   ├── main/
│   │   ├── java/com/taskmanager/
│   │   │   ├── config/
│   │   │   ├── controller/
│   │   │   ├── dto/
│   │   │   ├── entity/
│   │   │   ├── exception/
│   │   │   ├── repository/
│   │   │   ├── security/
│   │   │   ├── service/
│   │   │   └── TaskmanagerApiApplication.java
│   │   │
│   │   └── resources/
│   │       └── application.yml
│   │
│   └── test/
│
├── .gitignore
├── mvnw
├── mvnw.cmd
├── pom.xml
└── README.md
```

---

## 🔑 Authentication Flow

```text
1. User registers
       ↓
2. User logs in
       ↓
3. Server validates credentials
       ↓
4. JWT token is generated
       ↓
5. Client sends JWT with requests
       ↓
6. JWT Authentication Filter validates token
       ↓
7. Authorized request reaches controller
```

Protected requests use:

```http
Authorization: Bearer <JWT_TOKEN>
```

---

## 📌 API Endpoints

### Authentication

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Authenticate user and receive JWT |

### Tasks

| Method | Endpoint | Authentication |
|---|---|---|
| POST | `/api/tasks` | JWT Required |
| GET | `/api/tasks` | JWT Required |
| GET | `/api/tasks/{id}` | JWT Required |
| PUT | `/api/tasks/{id}` | JWT Required |
| DELETE | `/api/tasks/{id}` | JWT Required |

> The exact request/response models and available query parameters can be explored through Swagger.

---

## 📖 Swagger API Documentation

Once the application is running, open:

```text
http://localhost:8080/swagger-ui.html
```

OpenAPI specification:

```text
http://localhost:8080/v3/api-docs
```

Swagger provides an interactive interface for exploring and testing the REST APIs.

---

# ⚙️ Local Setup

## 1. Prerequisites

Install:

- Java 17+
- MySQL 8+
- Docker Desktop
- Git

Maven does not need to be installed globally because the project includes the Maven Wrapper.

Verify Java:

```bash
java -version
```

---

## 2. Clone the Repository

```bash
git clone https://github.com/tejaspande1211/spring-taskmanager-backend.git

cd spring-taskmanager-backend
```

---

## 3. Create MySQL Database

Open MySQL and run:

```sql
CREATE DATABASE taskmanager;
```

The application uses:

```text
Database: taskmanager
Host: localhost
Port: 3306
```

---

## 4. Configure Environment Variables

The application reads sensitive configuration from environment variables.

### Windows PowerShell

```powershell
$env:DB_USERNAME="root"
$env:DB_PASSWORD="your_mysql_password"
$env:JWT_SECRET="your_secure_jwt_secret"
```

### Linux / macOS

```bash
export DB_USERNAME="root"
export DB_PASSWORD="your_mysql_password"
export JWT_SECRET="your_secure_jwt_secret"
```

**Do not commit passwords, JWT secrets, or other credentials to GitHub.**

---

## 5. Start Redis

Run:

```bash
docker run -d --name redis-server -p 6379:6379 redis:7-alpine
```

Redis runs on:

```text
localhost:6379
```

---

## 6. Start RabbitMQ

Run:

```bash
docker run -d --name rabbitmq-server \
  -p 5672:5672 \
  -p 15672:15672 \
  rabbitmq:3-management
```

RabbitMQ:

```text
AMQP: localhost:5672
Management UI: http://localhost:15672
```

Default local development credentials:

```text
Username: guest
Password: guest
```

---

## 7. Run the Application

### Windows

```powershell
.\mvnw.cmd spring-boot:run
```

### Linux / macOS

```bash
./mvnw spring-boot:run
```

The API starts on:

```text
http://localhost:8080
```

---

## 🧪 Testing

The APIs can be tested using:

- Swagger UI
- Postman

Typical flow:

```text
Register User
     ↓
Login
     ↓
Receive JWT
     ↓
Authorize with JWT
     ↓
Create Task
     ↓
Get Tasks
     ↓
Update Task
     ↓
Delete Task
```

---

## 💡 Key Backend Concepts Demonstrated

This project provides practical experience with:

- REST API development
- Spring Boot application architecture
- Dependency Injection
- Spring Security
- JWT authentication
- Authentication filters
- DTO-based request handling
- Service and repository layers
- JPA/Hibernate
- MySQL database integration
- Redis caching
- RabbitMQ asynchronous messaging
- Input validation
- Exception handling
- API documentation with Swagger/OpenAPI
- Docker-based infrastructure
- Maven build and dependency management

---

## 🔮 Future Improvements

Potential improvements include:

- Docker Compose for the complete local environment
- Automated unit and integration tests
- CI/CD using GitHub Actions
- Role-based authorization
- Advanced task filtering
- Standardized API error responses
- Rate limiting
- AWS deployment
- React frontend
- File storage using Amazon S3

---

## 👨‍💻 Author

**Deepak Kumar**

Computer Science Engineering Graduate  
Backend / Software Developer

### Technologies of Interest

Java • Spring Boot • REST APIs • Spring Security • SQL • AWS • Docker • Redis • RabbitMQ • React