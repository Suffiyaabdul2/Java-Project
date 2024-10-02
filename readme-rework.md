# **Rewards Program API**

This project is a Spring Boot-based web application that provides an API to calculate rewards points for customers based on their transactions. MongoDB is used as the database to store the transaction details. Customers are awarded points based on the amount spent in each transaction:

- 2 points for every dollar spent over $100.
- 1 point for every dollar spent between $50 and $100.

## **Table of Contents**

1. [Features](#features)
2. [Technologies](#technologies)
3. [Setup Instructions](#setup-instructions)
4. [Running the Application](#running-the-application)
5. [API Endpoints](#api-endpoints)
6. [Database](#database)
7. [Testing](#testing)
8. [Notes](#notes)

---

## **Features**

- Retrieve reward points for a specific customer by month and year.
- Retrieve total reward points for a customer across all transactions.
- Fetch a list of distinct customer IDs.
- PostgreSQL is used for storage of transaction data.

---

## **Technologies**

- **Spring Boot 3.x**
- **PostgreSQL** (Database)
- **Java 17**
- **Spring Data JPA**
- **Maven** (Build Tool)
- **Postman** (API Testing)

---

## **Setup Instructions**

### 1. **Clone the Repository**

```bash
git clone https://github.com/your-repo/rewards-program-api.git
cd rewards-program-api
```

### 2. **Prerequisites**

- **Java 17** or higher installed.
- **PostgreSQL** installed and running.
- **Maven** installed.

### 3. **Configure MongoDB Connection**

In the `src/main/resources/application.properties` file, ensure the PostgreSQL connection string is properly set:

```properties
spring.data.mongodb.uri=postgresql://localhost:5432/rewardsdb
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
```

Update the username and password according to your PostgreSQL setup.

### 4. **Install Dependencies**

To install all necessary dependencies, run:

```bash
mvn clean install
```

### 5. **Run the Application**

Once dependencies are installed, you can start the application by running:

```bash
mvn spring-boot:run
```

The application will start on `http://localhost:8080`.

---

## **Running the Application**

After starting the application, you can interact with the API using tools like Postman or cURL.

Ensure that PostgreSQL is running before starting the application. 

---

## **API Endpoints**

### 1. **Get Monthly Reward Points for a Customer**

- **Description**: Fetch the reward points earned by a customer for a specific month and year.
- **URL**: `/api/rewards/{customerId}/monthly`
- **Method**: `GET`
- **Parameters**:
  - `customerId`: The ID of the customer (path variable).
  - `month`: The month in numeric format (query parameter).
  - `year`: The year in numeric format (query parameter).
- **Example**:
  ```http
  GET http://localhost:8080/api/rewards/1/monthly?month=8&year=2024
  ```
- **Response**:
  ```json
  {
    "points": 120
  }
  ```

### 2. **Get Total Reward Points for a Customer**

- **Description**: Fetch the total reward points earned by a customer across all transactions.
- **URL**: `/api/rewards/{customerId}/total`
- **Method**: `GET`
- **Parameters**:
  - `customerId`: The ID of the customer (path variable).
- **Example**:
  ```http
  GET http://localhost:8080/api/rewards/1/total
  ```
- **Response**:
  ```json
  {
    "points": 300
  }
  ```

### 3. **Get All Unique Customers**

- **Description**: Retrieve a list of all distinct customer IDs.
- **URL**: `/api/rewards/customers`
- **Method**: `GET`
- **Example**:
  ```http
  GET http://localhost:8080/api/rewards/customers
  ```
- **Response**:
  ```json
  [1, 2, 3, 4]
  ```

---

## **Database**

This application uses **PostgreSQL** as its database. All transaction records are stored in a PostgreSQL table called `transactions`.

### **Sample PostgreSQL Table Schema**:

```sql
CREATE TABLE transactions (
  id SERIAL PRIMARY KEY,
  customer_id INT NOT NULL,
  amount DECIMAL(10, 2) NOT NULL,
  transaction_date DATE NOT NULL
);
```
**Sample Database Record**

```sql
INSERT INTO transactions (customer_id, amount, transaction_date)
VALUES (1, 120.00, '2024-08-01');
```

---

## **Testing**

You can test the API using **Postman** or **cURL**.

### **Sample cURL Commands**:

1. **Get Monthly Rewards**:

   ```bash
   curl -X GET "http://localhost:8080/api/rewards/1/monthly?month=8&year=2024"
   ```

2. **Get Total Rewards**:

   ```bash
   curl -X GET "http://localhost:8080/api/rewards/1/total"
   ```

3. **Get All Customers**:
   ```bash
   curl -X GET "http://localhost:8080/api/rewards/customers"
   ```

---

## **Notes**

- Make sure PostgreSQL is running before starting the application.
- You can modify the `application.properties` file to change the PostgreSQL connection settings.
- Data is cleared and inserted during application startup for testing purposes. This behavior can be modified in the `DataLoader` class.

---

## **License**

This project is licensed under the MIT License.