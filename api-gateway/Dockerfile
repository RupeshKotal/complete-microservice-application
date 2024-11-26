# Stage-1 : buiild stage
FROM maven:3.9.4-eclipse-temurin-17 AS buiild

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests=true

#Stage-2: runtime Stage

FROM openjdk:17-alpine

WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

EXPOSE 8000

CMD ["java","-jar","app.jar"]