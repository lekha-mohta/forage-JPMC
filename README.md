# Midas Core - Transaction Processing System

A Spring Boot-based financial transaction processing system developed for the JPMC Advanced Software Engineering Forage program. This application handles user transactions through Kafka messaging, provides incentive calculations, and maintains user balances with real-time updates.

## Architecture Overview

The Midas Core system is built using a microservices architecture with the following key components:

- **Spring Boot Application**: Core transaction processing engine
- **Apache Kafka**: Message streaming for transaction events
- **H2 Database**: In-memory database for user records
- **REST API**: Balance inquiry endpoints
- **External Incentive Service**: Transaction reward calculations

## Features

- **Real-time Transaction Processing**: Kafka-based message consumption and processing
- **User Balance Management**: Automatic balance updates with transaction validation
- **Incentive Integration**: External service integration for transaction rewards
- **RESTful API**: Balance inquiry endpoints
- **Comprehensive Testing**: Multiple test scenarios for different use cases
- **Database Management**: JPA-based user record persistence

## Tech Stack

- **Java 17**
- **Spring Boot 3.2.5**
- **Spring Data JPA 3.2.5**
- **Spring Kafka 3.1.4**
- **H2 Database 2.2.224**
- **Maven 3.8.4**
- **JUnit 5** (for testing)

## Project Structure

```
src/
├── main/
│   ├── java/com/jpmc/midascore/
│   │   ├── MidasCoreApplication.java          # Main application entry point
│   │   ├── component/
│   │   │   └── DatabaseConduit.java           # Database operations wrapper
│   │   ├── config/
│   │   │   ├── KafkaTopicConfig.java          # Kafka topic configuration
│   │   │   └── RestConfig.java                # REST template configuration
│   │   ├── controller/
│   │   │   └── BalanceController.java         # REST API for balance queries
│   │   ├── entity/
│   │   │   └── UserRecord.java                # User entity model
│   │   ├── foundation/
│   │   │   ├── Balance.java                   # Balance response model
│   │   │   ├── Incentive.java                 # Incentive model
│   │   │   └── Transaction.java               # Transaction model
│   │   ├── kafka/
│   │   │   ├── KafkaJSONConsumer.java         # Transaction message consumer
│   │   │   └── KafkaJSONProducer.java         # Message producer
│   │   ├── payload/
│   │   │   └── TransactionResponse.java       # Transaction response wrapper
│   │   ├── repository/
│   │   │   └── UserRepository.java            # User data repository
│   │   └── service/
│   │       ├── TransactionService.java        # Transaction business logic
│   │       ├── UserService.java               # User management service
│   │       ├── RestTemplateService.java       # External service integration
│   │       └── impl/                          # Service implementations
│   └── resources/
│       ├── application.yml                    # Application configuration
│       └── test_data/                         # Test data files
└── test/
    ├── java/com/jpmc/midascore/
    │   ├── FileLoader.java                    # Test utility for loading data
    │   ├── KafkaProducer.java                 # Test Kafka producer
    │   ├── UserPopulator.java                 # Test user data setup
    │   ├── BalanceQuerier.java                # Balance API test client
    │   └── Task*Tests.java                    # Test scenarios (1-5)
    └── resources/test_data/                   # Test data files
```

## Getting Started

### Prerequisites

- Java 17 or higher
- Maven 3.6+
- Apache Kafka (for production use)

### Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd forage-JPMC
   ```

2. **Build the project:**
   ```bash
   ./mvnw clean compile
   ```

3. **Run tests:**
   ```bash
   ./mvnw test
   ```

4. **Start the application:**
   ```bash
   ./mvnw spring-boot:run
   ```

The application will start on `http://localhost:33400`

## Testing Scenarios

The project includes comprehensive test scenarios:

### Task 1: Application Boot Test
- **File**: [`TaskOneTests.java`](src/test/java/com/jpmc/midascore/TaskOneTests.java)
- **Purpose**: Validates application startup and basic functionality
- **Output**: Mathematical sequence verification

### Task 2: Transaction Processing Test
- **File**: [`TaskTwoTests.java`](src/test/java/com/jpmc/midascore/TaskTwoTests.java)
- **Data**: [`poiuytrewq.uiop`](src/test/resources/test_data/poiuytrewq.uiop)
- **Purpose**: Tests transaction message consumption and processing

### Task 3: User Balance Verification
- **File**: [`TaskThreeTests.java`](src/test/java/com/jpmc/midascore/TaskThreeTests.java)
- **Data**: [`mnbvcxz.vbnm`](src/test/resources/test_data/mnbvcxz.vbnm) + [`lkjhgfdsa.hjkl`](src/test/resources/test_data/lkjhgfdsa.hjkl)
- **Purpose**: Validates balance calculations after multiple transactions

### Task 4: Advanced Balance Testing
- **File**: [`TaskFourTests.java`](src/test/java/com/jpmc/midascore/TaskFourTests.java)
- **Data**: [`alskdjfh.fhdjsk`](src/test/resources/test_data/alskdjfh.fhdjsk)
- **Purpose**: Tests complex transaction scenarios

### Task 5: API Integration Test
- **File**: [`TaskFiveTests.java`](src/test/java/com/jpmc/midascore/TaskFiveTests.java)
- **Data**: [`rueiwoqp.tyruei`](src/test/resources/test_data/rueiwoqp.tyruei)
- **Purpose**: Tests REST API balance queries and external service integration

### Running Specific Tests

```bash
# Run a specific test class
./mvnw test -Dtest=TaskOneTests

# Run all tests
./mvnw test
```

## API Endpoints

### Balance Inquiry
- **Endpoint**: `GET /balance?userId={userId}`
- **Description**: Retrieves the current balance for a specific user
- **Response**: JSON object with balance amount
- **Example**: 
  ```bash
  curl "http://localhost:33400/balance?userId=1"
  ```
  ```json
  {
    "amount": 1234.56
  }
  ```

## Transaction Flow

1. **Transaction Input**: Transaction data is published to Kafka topic
2. **Message Consumption**: [`KafkaJSONConsumer`](src/main/java/com/jpmc/midascore/kafka/KafkaJSONConsumer.java) receives transaction messages
3. **Validation**: Transaction service validates sender balance and user existence
4. **Processing**: Money transfer between sender and recipient
5. **Incentive Calculation**: External service call for transaction rewards
6. **Balance Update**: User balances updated with transaction amount and incentives
7. **Persistence**: Changes saved to database

## Data Models

### User Record
```java
// User entity with auto-generated ID
{
  "id": 1,
  "name": "john_doe",
  "balance": 1500.75
}
```

### Transaction
```java
// Transaction message format
{
  "senderId": 1,
  "recipientId": 2,
  "amount": 100.50,
  "incentive": {
    "amount": 5.25
  }
}
```

### Test Data Format

**User Data** (`.hjkl` files):
```
username, initial_balance
bernie, 1200.23
maria, 2774.14
```

**Transaction Data** (`.uiop`, `.vbnm`, `.fhdjsk`, `.tyruei` files):
```
sender_id, recipient_id, amount
1, 2, 100.50
3, 1, 75.25
```

## Debugging & Development

### Debug Mode
The test classes include debug-friendly infinite loops with logging to help trace transaction processing:

```java
logger.info("use your debugger to watch for incoming transactions");
while (true) {
    Thread.sleep(20000);
    logger.info("...");
}
```

### Logging
- Set breakpoints in [`TransactionServiceImpl`](src/main/java/com/jpmc/midascore/service/impl/TransactionServiceImpl.java) to trace transaction flow
- Monitor Kafka consumer logs in [`KafkaJSONConsumer`](src/main/java/com/jpmc/midascore/kafka/KafkaJSONConsumer.java)
- Check balance calculations in [`BalanceController`](src/main/java/com/jpmc/midascore/controller/BalanceController.java)

## Development Notes

- **Database**: Uses H2 in-memory database (resets on application restart)
- **Kafka**: Embedded Kafka for testing, external Kafka for production
- **Transactions**: Database transactions ensure consistency
- **Error Handling**: Comprehensive validation for transaction processing
- **Testing**: Uses Spring Boot Test with embedded Kafka

## Dependencies

Key dependencies from [`pom.xml`](pom.xml):

- Spring Boot Starter Data JPA (3.2.5)
- Spring Boot Starter Web (3.2.5)
- Spring Kafka (3.1.4)
- H2 Database (2.2.224)
- Spring Boot Starter Test (3.2.5)
- Spring Kafka Test (3.1.4)
- Testcontainers Kafka (1.19.1)

## Contributing

This is a learning project for the JPMC Forage program. Follow these steps for development:

1. Ensure all tests pass before making changes
2. Add appropriate test coverage for new features
3. Follow the existing code structure and naming conventions
4. Update documentation for significant changes

## License

This project is part of the JPMC Advanced Software Engineering Forage program.

