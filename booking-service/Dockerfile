FROM maven:3.9-eclipse-temurin-25 AS build

WORKDIR /app

COPY pom.xml .
RUN --mount=type=cache,target=/root/.m2 \
    mvn -B -q dependency:resolve

COPY src/ src/
RUN --mount=type=cache,target=/root/.m2 \
    mvn -B -DskipTests package


FROM eclipse-temurin:25-jre-jammy AS runtime

WORKDIR /app

RUN groupadd --gid 1001 appgroup && \
    useradd --uid 1001 --gid appgroup --no-create-home appuser

COPY --from=build /app/target/LinguaRide-0.0.1-SNAPSHOT.jar /app/app.jar

USER appuser

ENTRYPOINT ["java", \
    "-XX:+UseContainerSupport", \
    "-XX:MaxRAMPercentage=75", \
    "-XX:+ExitOnOutOfMemoryError", \
    "-jar", "/app/app.jar"]