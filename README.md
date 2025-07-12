# Card Service - Spring Boot on Google Cloud Run

A Spring Boot microservice for managing card issuance, designed to run on Google Cloud Run with PostgreSQL database integration.

## Overview

This project demonstrates a cloud-native Spring Boot application that provides REST APIs for card management operations. The service is containerized using Google Jib and deployed to Google Cloud Run with Cloud SQL PostgreSQL integration.

## Features

- **Card Management**: Issue new cards and retrieve card statistics
- **Cloud-Native**: Designed for Google Cloud Run deployment
- **Database Integration**: PostgreSQL with Cloud SQL connector
- **Containerization**: Docker images built with Google Jib
- **CI/CD**: GitHub Actions workflows for automated deployment
- **Health Monitoring**: Spring Boot Actuator endpoints

## Technology Stack

- **Framework**: Spring Boot 2.7.2
- **Language**: Java 17
- **Database**: PostgreSQL
- **Build Tool**: Gradle
- **Containerization**: Google Jib
- **Cloud Platform**: Google Cloud Platform
- **Deployment**: Google Cloud Run
- **Database**: Google Cloud SQL (PostgreSQL)

## API Endpoints

### Card Operations
- `POST /card-service/cards` - Issue a new card
- `GET /card-service/cards/count` - Get total card count

### Health Check
- `GET /card-service/actuator/health` - Application health status

## Project Structure

```
card-service-cloudrun/
├── src/
│   ├── main/
│   │   ├── java/vn/cloud/cardservice/
│   │   │   └── CardServiceApplication.java
│   │   └── resources/
│   │       └── application.yml
│   └── test/
├── .github/workflows/
│   ├── CI-workflow.yml
│   └── google-cloudrun.yml
├── build.gradle
├── gradlew
└── gradlew.bat
```

## Getting Started

### Prerequisites

- Java 17 or higher
- Gradle (or use included wrapper)
- PostgreSQL database
- Google Cloud SDK (for deployment)

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd GCP-Springboot/card-service-cloudrun
   ```

2. **Set up environment variables**
   ```bash
   export DB_DATABASE=your_database_name
   export DB_USERNAME=your_username
   export DB_PASSWORD=your_password
   export DB_CLOUD_SQL_INSTANCE=your_project:region:instance_name
   ```

3. **Run the application**
   ```bash
   ./gradlew bootRun
   ```

4. **Test the API**
   ```bash
   # Issue a new card
   curl -X POST http://localhost:8080/card-service/cards \
     -H "Content-Type: application/json" \
     -d '{"card":"1234-5678-9012-3456","description":"Test Card"}'

   # Get card count
   curl http://localhost:8080/card-service/cards/count
   ```

### Building the Application

```bash
# Build the application
./gradlew build

# Build Docker image with Jib
./gradlew jib
```

## Configuration

The application uses the following configuration in `application.yml`:

- **Server Context Path**: `/card-service`
- **Database**: PostgreSQL with Cloud SQL Socket Factory
- **Connection Pool**: HikariCP with optimized settings for Cloud Run
- **JPA**: Hibernate with automatic DDL updates

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DB_DATABASE` | Database name | `postgres` |
| `DB_USERNAME` | Database username | `postgres` |
| `DB_PASSWORD` | Database password | `postgres` |
| `DB_CLOUD_SQL_INSTANCE` | Cloud SQL instance connection name | `card-connection-name` |

## Deployment

### Google Cloud Run

The project includes GitHub Actions workflows for automated deployment:

1. **CI Workflow**: Runs tests and builds the application
2. **Cloud Run Deployment**: Builds container image and deploys to Cloud Run

### Manual Deployment

1. **Build and push container image**
   ```bash
   ./gradlew jib --image=gcr.io/YOUR_PROJECT_ID/card-service
   ```

2. **Deploy to Cloud Run**
   ```bash
   gcloud run deploy card-service \
     --image gcr.io/YOUR_PROJECT_ID/card-service \
     --platform managed \
     --region asia-east1 \
     --allow-unauthenticated
   ```

## Database Schema

The application automatically creates the following table:

```sql
CREATE TABLE card (
    id BIGSERIAL PRIMARY KEY,
    card VARCHAR(255),
    description VARCHAR(255)
);
```

## Monitoring and Health Checks

- **Health Endpoint**: `/card-service/actuator/health`
- **Application Metrics**: Available through Spring Boot Actuator
- **Logging**: Structured logging with SLF4J

## Development Notes

- Uses Lombok for reducing boilerplate code
- Implements Repository pattern with Spring Data JPA
- RESTful API design with proper HTTP methods
- Environment-based configuration for different deployment stages

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is part of a learning exercise for Google Cloud Platform and Spring Boot integration.
