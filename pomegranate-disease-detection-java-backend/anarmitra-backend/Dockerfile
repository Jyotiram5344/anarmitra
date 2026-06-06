# ==========================================
# Stage 1: Build the application
# ==========================================
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /app

# Copy pom.xml first to download dependencies and cache them
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code and build the executable jar file
COPY src ./src
RUN mvn clean package -DskipTests

# ==========================================
# Stage 2: Run the application
# ==========================================
FROM eclipse-temurin:21-jre-jammy
WORKDIR /app

# Copy the built jar from the build stage to the run stage
# Wildcard is used to match the jar dynamically regardless of project version
COPY --from=build /app/target/*.jar app.jar

# Expose port 8080 (the default port, which will be dynamically overridden by Render using the PORT environment variable)
EXPOSE 8080

# Run the application with optimized memory settings if needed, reading environment variables at runtime
ENTRYPOINT ["java", "-jar", "app.jar"]
