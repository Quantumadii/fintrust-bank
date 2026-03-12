# Stage 1: Build
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy the common-lib and transaction-service folders
COPY common-lib /app/common-lib
COPY transaction-service /app/transaction-service

# Install common-lib to the local container repo
RUN mvn -f /app/common-lib/pom.xml clean install -DskipTests

# Build the Transaction Service
RUN mvn -f /app/transaction-service/pom.xml clean package -DskipTests

# Stage 2: Runtime
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /app/transaction-service/target/*.jar app.jar

EXPOSE 10000
ENTRYPOINT ["java", "-jar", "app.jar"]
