# Stage 1: Build
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy the common-lib folder AND the service folder
# (Assumes your repo has these as sibling folders)
COPY common-lib /app/common-lib
COPY auth-service /app/auth-service

# Build common-lib first so it's available in the local .m2 repo
RUN mvn -f /app/common-lib/pom.xml clean install -DskipTests

# Build the actual service
RUN mvn -f /app/auth-service/pom.xml clean package -DskipTests

# Stage 2: Runtime
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /app/auth-service/target/*.jar app.jar

EXPOSE 10000
ENTRYPOINT ["java", "-jar", "app.jar"]
