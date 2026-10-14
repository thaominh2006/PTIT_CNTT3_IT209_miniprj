# ===== Giai đoạn 1: build file jar =====
FROM eclipse-temurin:21-jdk AS build
WORKDIR /workspace

# Copy Gradle wrapper và file cấu hình trước để tận dụng cache layer
COPY gradlew settings.gradle build.gradle ./
COPY gradle ./gradle
RUN chmod +x gradlew && ./gradlew --no-daemon dependencies > /dev/null 2>&1 || true

# Copy source và đóng gói (kiểm thử đã chạy riêng bằng ./gradlew test)
COPY src ./src
RUN ./gradlew --no-daemon bootJar -x test \
    && cp build/libs/*.jar /workspace/app.jar

# ===== Giai đoạn 2: image chạy ứng dụng =====
FROM eclipse-temurin:21-jre
WORKDIR /app

# curl dùng cho healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends curl \
    && rm -rf /var/lib/apt/lists/*

# Chạy bằng user thường, không dùng root
RUN useradd --system --create-home appuser
COPY --from=build /workspace/app.jar /app/app.jar
USER appuser

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app/app.jar"]