# ====== Stage 1: Build WAR ======
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Install git (required to clone repo)
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

# Clone your repository
RUN git clone https://github.com/<username>/<repository>.git .

# Copy pom.xml first for caching dependencies (optional if repo already has pom.xml)
COPY pom.xml .
RUN mvn dependency:go-offline

# Build the WAR
RUN mvn clean package -DskipTests

# ====== Stage 2: Tomcat Deployment ======
FROM tomcat:10.1.16-jdk17-temurin
WORKDIR /usr/local/tomcat/webapps

# Copy WAR from build stage WITHOUT renaming
COPY --from=build /app/target/*.war ./

# Expose Tomcat port
EXPOSE 8080

# Start Tomcat
CMD ["catalina.sh", "run"]
