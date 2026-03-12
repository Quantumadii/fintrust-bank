# Stage 1: Build the shared library and the Gateway
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# 1. Copy the common-lib folder
# This is necessary because the Gateway depends on your ApiResponse DTO
COPY common-lib /app/common-lib

# 2. Copy the api-gateway folder
COPY api-gateway /app/api-gateway

# 3. Install common-lib to the local Maven repository inside the container
# This makes your DTOs available for the next step
RUN mvn -f /app/common-lib/pom.xml clean install -DskipTests

# 4. Build the API Gateway
RUN mvn -f /app/api-gateway/pom.xml clean package -DskipTests

# Stage 2: Create the lightweight runtime image
FROM eclipse-temurin:17-jre
WORKDIR /app

# Copy the built JAR file from the build stage
COPY --from=build /app/api-gateway/target/*.jar app.jar

# Render expects the service to listen on port 10000
EXPOSE 10000

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
