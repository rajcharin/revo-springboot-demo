# Revo Spring Boot Demo

[![Java](https://img.shields.io/badge/Java-8-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.3.0-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Maven](https://img.shields.io/badge/Maven-3.6+-blue.svg)](https://maven.apache.org/)
[![License](https://img.shields.io/badge/License-Internal-red.svg)]()

A production-grade Spring Boot application demonstrating enterprise-level DevSecOps practices, security implementation, and cloud-native architecture.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Testing](#testing)
- [Security](#security)
- [API Documentation](#api-documentation)
- [Deployment](#deployment)
- [Monitoring](#monitoring)
- [Contributing](#contributing)
- [Documentation](#documentation)

## Overview

This is a Proof of Concept (POC) Spring Boot application built to demonstrate production-ready development practices with a comprehensive DevSecOps pipeline. The application showcases enterprise patterns, security best practices, and automated deployment strategies.

**Key Highlights:**
- Production-grade architecture
- Integrated DevSecOps pipeline (CI/CD)
- Multi-environment configuration
- Comprehensive security implementation
- Cloud-native design

For detailed POC documentation and DevSecOps pipeline information, see [claude.md](./claude.md).

## Features

- **RESTful API**: Well-structured REST endpoints
- **Spring Security**: Authentication and authorization
- **JPA/Hibernate**: Database ORM integration
- **Multi-Environment**: Dev, Staging, and Production profiles
- **Health Checks**: Actuator endpoints for monitoring
- **MySQL Integration**: Production-ready database setup
- **Security Scanning**: OWASP, Snyk, and Trivy integration
- **Automated Testing**: Unit and integration tests
- **Docker Support**: Containerized deployment
- **CI/CD Pipeline**: Automated build, test, and deployment

## Technology Stack

| Component | Technology | Version |
|-----------|-----------|---------|
| **Framework** | Spring Boot | 2.3.0 |
| **Language** | Java | 8 |
| **Build Tool** | Maven | 3.6+ |
| **Database** | MySQL | 5.7+/8.0 |
| **Security** | Spring Security | 5.3.x |
| **Monitoring** | Spring Actuator | 2.3.x |
| **Testing** | JUnit | 5.x |
| **Container** | Docker | 20.x+ |
| **Orchestration** | Kubernetes | 1.20+ |

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- **Java Development Kit (JDK) 8 or higher**
  ```bash
  java -version
  ```

- **Maven 3.6 or higher**
  ```bash
  mvn -version
  ```

- **MySQL 5.7+ or 8.0**
  ```bash
  mysql --version
  ```

- **Docker** (optional, for containerized setup)
  ```bash
  docker --version
  ```

- **Git**
  ```bash
  git --version
  ```

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/rajcharin/revo-springboot-demo.git
   cd revo-springboot-demo
   ```

2. **Configure database** (see [Configuration](#configuration) section)

3. **Build the application**
   ```bash
   mvn clean package
   ```

4. **Run the application**
   ```bash
   mvn spring-boot:run
   ```

5. **Access the application**
   ```
   http://localhost:8080
   ```

6. **Check health status**
   ```bash
   curl http://localhost:8080/actuator/health
   ```

## Project Structure

```
revo-springboot-demo/
├── .mvn/                       # Maven wrapper files
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/revotive/demo/
│   │   │       ├── Application.java      # Main application class
│   │   │       ├── DemoApplication.java  # Demo application
│   │   │       ├── controller/           # REST controllers
│   │   │       ├── service/              # Business logic
│   │   │       ├── repository/           # Data access layer
│   │   │       ├── model/                # Entity classes
│   │   │       ├── dto/                  # Data transfer objects
│   │   │       ├── config/               # Configuration classes
│   │   │       └── security/             # Security configuration
│   │   └── resources/
│   │       ├── application.properties           # Default configuration
│   │       ├── application-dev.properties       # Development config
│   │       ├── application-prod.properties      # Production config
│   │       ├── log4j.properties                 # Logging configuration
│   │       └── static/                          # Static resources
│   └── test/
│       └── java/
│           └── com/revotive/demo/
│               └── DemoApplicationTests.java    # Test classes
├── .gitignore
├── pom.xml                     # Maven dependencies
├── README.md                   # This file
├── claude.md                   # POC & DevSecOps documentation
├── Dockerfile                  # Docker image definition
└── docker-compose.yml          # Multi-container setup

```

## Configuration

### Database Configuration

#### Development Environment

Update `src/main/resources/application-dev.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/revo_dev?useSSL=false&serverTimezone=UTC
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
```

#### Production Environment

Update `src/main/resources/application-prod.properties`:

```properties
spring.datasource.url=jdbc:mysql://your-production-host:3306/revo_prod?useSSL=true&serverTimezone=UTC
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
spring.jpa.hibernate.ddl-auto=validate
```

**Note**: Use environment variables for sensitive data in production.

### Using Docker Compose for Local Development

Create a `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: revo-mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: revo_dev
      MYSQL_USER: revouser
      MYSQL_PASSWORD: revopass
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

Start the database:
```bash
docker-compose up -d
```

## Running the Application

### Development Mode

Run with the development profile (default):

```bash
mvn spring-boot:run
```

Or specify the profile explicitly:

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=dev
```

### Production Mode

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=prod
```

### As a JAR/WAR

Build the package:

```bash
mvn clean package
```

Run the WAR file:

```bash
java -jar target/revo-demo.war
```

### With Docker

Build the Docker image:

```bash
docker build -t revo-demo:latest .
```

Run the container:

```bash
docker run -p 8080:8080 \
  -e SPRING_PROFILES_ACTIVE=prod \
  -e DB_USERNAME=your_user \
  -e DB_PASSWORD=your_password \
  revo-demo:latest
```

## Testing

### Run All Tests

```bash
mvn test
```

### Run Integration Tests

```bash
mvn verify
```

### Generate Code Coverage Report

```bash
mvn clean test jacoco:report
```

View the report:
```bash
open target/site/jacoco/index.html
```

### Run Specific Test Class

```bash
mvn test -Dtest=DemoApplicationTests
```

## Security

### Security Features

- **Authentication**: Spring Security with JWT/Session-based auth
- **Authorization**: Role-based access control (RBAC)
- **HTTPS**: TLS/SSL enabled in production
- **CSRF Protection**: Enabled for state-changing operations
- **SQL Injection Prevention**: Parameterized queries via JPA
- **XSS Protection**: Content Security Policy headers
- **Password Encryption**: BCrypt password hashing

### Security Scanning

Run OWASP Dependency Check:

```bash
mvn org.owasp:dependency-check-maven:check
```

View the security report:
```bash
open target/dependency-check-report.html
```

### Environment Variables

Store sensitive information in environment variables:

```bash
export DB_USERNAME=your_username
export DB_PASSWORD=your_password
export JWT_SECRET=your_secret_key
```

**Never commit credentials to version control!**

## API Documentation

### Health Check Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/actuator/health` | GET | Application health status |
| `/actuator/info` | GET | Application information |
| `/actuator/metrics` | GET | Application metrics |

### Example Request

```bash
curl -X GET http://localhost:8080/actuator/health
```

Response:
```json
{
  "status": "UP"
}
```

### Swagger/OpenAPI (Optional)

If Swagger is configured, access the API documentation at:
```
http://localhost:8080/swagger-ui.html
```

## Deployment

### Deployment Environments

| Environment | URL | Purpose |
|-------------|-----|---------|
| **Development** | http://localhost:8080 | Local development |
| **Staging** | https://staging.example.com | Pre-production testing |
| **Production** | https://app.example.com | Live application |

### CI/CD Pipeline

The application uses a comprehensive DevSecOps pipeline:

1. **Build & Test**: Maven build with unit and integration tests
2. **Security Scan**: OWASP, Snyk, Trivy vulnerability scanning
3. **Code Quality**: SonarQube analysis
4. **Container Build**: Docker image creation
5. **Deploy**: Kubernetes deployment with Helm
6. **Monitor**: Prometheus and Grafana monitoring

See [claude.md](./claude.md) for complete pipeline documentation.

### Manual Deployment to Kubernetes

```bash
# Build Docker image
docker build -t revo-demo:v1.0.0 .

# Push to registry
docker tag revo-demo:v1.0.0 registry.example.com/revo-demo:v1.0.0
docker push registry.example.com/revo-demo:v1.0.0

# Deploy to Kubernetes
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

# Check deployment status
kubectl rollout status deployment/revo-demo

# View logs
kubectl logs -f deployment/revo-demo
```

## Monitoring

### Application Monitoring

Access Spring Boot Actuator endpoints:

- **Health**: `http://localhost:8080/actuator/health`
- **Metrics**: `http://localhost:8080/actuator/metrics`
- **Info**: `http://localhost:8080/actuator/info`

### Production Monitoring Stack

- **Prometheus**: Metrics collection
- **Grafana**: Visualization dashboards
- **ELK Stack**: Log aggregation (Elasticsearch, Logstash, Kibana)
- **Jaeger**: Distributed tracing
- **PagerDuty**: Alerting and on-call management

### Key Metrics to Monitor

- Request throughput (requests/sec)
- Response time (p50, p95, p99)
- Error rate (4xx, 5xx)
- JVM metrics (heap, GC, threads)
- Database connection pool usage
- CPU and memory utilization

## Contributing

### Development Workflow

1. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**
   - Follow coding standards
   - Write tests for new features
   - Update documentation

3. **Run tests**
   ```bash
   mvn clean verify
   ```

4. **Commit your changes**
   ```bash
   git add .
   git commit -m "feat: add your feature description"
   ```

5. **Push to remote**
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create a Pull Request**
   - Provide a clear description
   - Link related issues
   - Request code review

### Code Standards

- Follow Java coding conventions
- Use meaningful variable and method names
- Write JavaDoc for public APIs
- Maintain test coverage > 80%
- Pass all quality gates (SonarQube)

### Commit Message Format

```
<type>: <subject>

<body>

<footer>
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Example:
```
feat: add user authentication endpoint

Implement JWT-based authentication for user login.
Includes password encryption and token generation.

Closes #123
```

## Documentation

### Additional Documentation

- **[claude.md](./claude.md)**: Complete POC and DevSecOps pipeline documentation
- **[API Documentation](./docs/api.md)**: Detailed API specifications (if available)
- **[Architecture](./docs/architecture.md)**: System architecture diagrams (if available)
- **[Deployment Guide](./docs/deployment.md)**: Deployment procedures (if available)

### Useful Commands

```bash
# Clean build
mvn clean

# Compile only
mvn compile

# Package without tests
mvn package -DskipTests

# Run with debug
mvn spring-boot:run -Dspring-boot.run.jvmArguments="-Xdebug"

# Check for dependency updates
mvn versions:display-dependency-updates

# Generate project site
mvn site
```

## Troubleshooting

### Common Issues

**Issue**: Database connection failed
- **Solution**: Check MySQL is running and credentials are correct in application properties

**Issue**: Port 8080 already in use
- **Solution**: Change port in `application.properties`: `server.port=8081`

**Issue**: Build fails with compilation errors
- **Solution**: Ensure JDK 8+ is installed and JAVA_HOME is set correctly

**Issue**: Tests fail
- **Solution**: Check database configuration in test profile

### Getting Help

- Check the [claude.md](./claude.md) documentation
- Review application logs in `logs/` directory
- Contact the development team
- Raise an issue in the repository

## License

Internal Use Only - All Rights Reserved

---

## Project Status

**Status**: POC Complete ✅

**Last Updated**: 2025-10-22

**Maintained By**: Revotive Development Team

---

**Happy Coding!** 🚀
