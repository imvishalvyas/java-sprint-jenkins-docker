# Stage 1: Build the JAR using Maven
FROM maven:3.9.9-eclipse-temurin-21 AS build

# Set working dir inside container
WORKDIR /app

# Copy pom.xml and download dependencies first (better cache)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Build application
RUN mvn clean package -DskipTests

# Stage 2: Run the app with JDK
FROM openjdk:21-jdk-slim

WORKDIR /app

# Copy the built JAR from previous stage
COPY --from=build /app/target/spring-boot-complete-0.0.1-SNAPSHOT.jar app.jar

# Expose app port
EXPOSE 8080

# Start the app
ENTRYPOINT ["java", "-jar", "app.jar"]

