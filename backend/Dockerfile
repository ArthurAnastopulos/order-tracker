# --- Stage 1: Build ---
FROM openjdk:21-slim AS build

WORKDIR /workspace

# Instala dependências básicas
RUN apt-get update && apt-get install -y curl unzip

# Baixa e instala Gradle manualmente (exemplo versão 8.3)
RUN curl -fsSL https://services.gradle.org/distributions/gradle-8.3-bin.zip -o gradle.zip \
    && unzip gradle.zip -d /opt \
    && rm gradle.zip

ENV GRADLE_HOME=/opt/gradle-8.3
ENV PATH=${GRADLE_HOME}/bin:${PATH}

COPY build.gradle settings.gradle ./
COPY src ./src

RUN gradle build --no-daemon -x test

# --- Stage 2: Runtime ---
FROM openjdk:21-slim AS runtime

WORKDIR /app

COPY --from=build /workspace/build/libs/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
