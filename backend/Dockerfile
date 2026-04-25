# 1) Build stage
FROM maven:3.9-eclipse-temurin-21 AS builder
WORKDIR /app

# Copy Maven files first (better caching)
COPY pom.xml .
RUN mvn -q -DskipTests dependency:go-offline

# Copy source and build jar
COPY src ./src
RUN mvn -q -DskipTests clean package

# 2) Run stage
FROM eclipse-temurin:21-jre
WORKDIR /app

# Copy built jar from builder stage
COPY --from=builder /app/target/*.jar app.jar

# Backend port
EXPOSE 8085

# Start Spring Boot app
ENTRYPOINT ["java", "-jar", "app.jar"]