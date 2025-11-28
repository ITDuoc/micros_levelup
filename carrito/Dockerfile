# Stage 1: build
FROM eclipse-temurin:17-jdk-focal AS build
WORKDIR /app

COPY pom.xml mvnw ./
COPY .mvn .mvn
RUN ./mvnw -q -B -ntp -DskipTests dependency:go-offline

COPY src ./src
RUN ./mvnw -q -B -ntp -DskipTests package

# Stage 2: runtime
FROM eclipse-temurin:17-jre-focal
WORKDIR /app

# Crear usuario no-root (compatible con Debian/Ubuntu)
RUN groupadd --system app && useradd --system --gid app app

COPY --from=build /app/target/*.jar app.jar

USER app
EXPOSE 8082

ENTRYPOINT ["java","-Xms256m","-Xmx512m","-jar","/app/app.jar"]
